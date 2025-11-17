# Running Your RPC Node

This guide covers deploying Gorchain nodes (RPC and monitoring) on production infrastructure using stack-orchestrator.

## Prerequisites

* `laconic-so` CLI installed
* Gorchain stacks repository cloned to `~/workspace/gorchain-stacks/` ([GitHub](https://github.com/gorbagana-dev/gorchain-stacks))
* SSL certificate and private key for envoy proxy (RPC node)

## Environment Setup

Set the stacks directory path:

```bash
stacks=~/workspace/gorchain-stacks/stack-orchestrator/stacks/
```

Create a directory to store persistent deployment files:

```bash
mkdir ~/so-deploy
```

## RPC Node Deployment

### 1. Get Validator Identity

First, extract the validator's pubkey identity:

```bash
export KNOWN_VALIDATOR=$(docker exec $(docker ps -qf label='role=validator') \
  solana-keygen pubkey /agave/config/validator-identity.json)
```

### 2. Build RPC Images

```bash
laconic-so --stack $stacks/gorchain-rpc build-containers
```

### 3. Initialize RPC Deployment

Create spec files and deployment directory, passing certificate and private key for envoy:

```bash
rpc_spec=~/so-deploy/rpc-spec.yml
rpc_deployment=~/so-deploy/rpc-deployment

laconic-so --stack $stacks/gorchain-rpc deploy init --output $rpc_spec
laconic-so --stack $stacks/gorchain-rpc deploy --cluster gorchain create \
  --spec-file $rpc_spec \
  --deployment-dir $rpc_deployment \
  -- \
  --certificate-file ~/workspace/agave-gor-fork/origin.cert.pem \
  --private-key-file ~/workspace/agave-gor-fork/private_key_for_origin_cert
```

### 4. Configure RPC Environment

**Note:**

* Docker does not automatically create firewall rules for host network mode, so ports must be manually allowed in the firewall.
* The gorchain and gorchain-rpc stacks both run on the host network, so ports cannot conflict if running on the same host.
* If running on an external host, ensure RPC ports, gossip, and the dynamic port range are all accessible.
* Set `PUBLIC_RPC_ADDRESS` as needed for external access. If this is not set, the node will be started in private RPC node and not discoverable.

```bash
cat > $rpc_deployment/config.env <<EOF
export RPC_PORT=8899
export RPC_WS_PORT=8900
export GOSSIP_PORT=8001
export KNOWN_VALIDATOR=$KNOWN_VALIDATOR
export VALIDATOR_ENTRYPOINT=gorbagana.vaasl.io:8001
EOF
```

### 5. Start the RPC Node

```bash
laconic-so deployment --dir $rpc_deployment start
```

**OR** run it like this:

```bash
laconic-so --stack ~/workspace/gorchain-stacks/stack-orchestrator/stacks/gorchain-rpc deploy --env-file ~/so-deploy/rpc-deployment/config.env up
```

## Monitoring Deployment

There are no images to build for monitoring, so directly create the deployment.

### 1. Initialize Monitoring Deployment

```bash
monitoring_spec=~/so-deploy/monitoring-spec.yml
monitoring_deployment=~/so-deploy/monitoring-deployment

laconic-so --stack $stacks/gorchain-monitoring deploy init --output $monitoring_spec
laconic-so --stack $stacks/gorchain-monitoring deploy create \
  --spec-file $monitoring_spec \
  --deployment-dir $monitoring_deployment
```

### 2. Start InfluxDB

Only InfluxDB needs to run on the deployment to ingest data. The stack contains Grafana and other services which can be run locally against a local or remote InfluxDB.

```bash
laconic-so deployment --dir $monitoring_deployment start influxdb
```

### 3. Initialize InfluxDB

After starting InfluxDB, run the custom setup script to initialize it by mapping node names to pubkeys:

```bash
laconic-so --stack $stacks/gorchain-monitoring deploy setup
```

This discovers running Agave containers, extracts their identity pubkeys, and stores the mappings in InfluxDB for Grafana dashboard selectors.
