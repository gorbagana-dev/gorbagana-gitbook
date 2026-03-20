# Troubleshooting

This section covers common issues you may encounter when setting up and running your RPC node, along with solutions to resolve them.

## Common Issues

### Installation Problems

#### Issue: `laconic-so` command not found

**Solution:**
* Ensure `laconic-so` is in your PATH environment variable
* Verify the installation directory is included in your PATH:
  ```bash
  echo $PATH
  ```
* Add the directory to your PATH if needed:
  ```bash
  export PATH=$PATH:~/bin
  ```
* Add this line to your `~/.bashrc` or `~/.zshrc` to make it permanent

#### Issue: Docker permission denied

**Solution:**
* Add your user to the docker group:
  ```bash
  sudo usermod -aG docker $USER
  ```
* Log out and log back in, or run:
  ```bash
  newgrp docker
  ```

### Deployment Issues

#### Issue: Port conflicts when starting RPC node

**Symptoms:** Error messages about ports already in use

**Solution:**
* Check which process is using the port:
  ```bash
  sudo lsof -i :8899  # Replace with your RPC_PORT
  sudo lsof -i :8900  # Replace with your RPC_WS_PORT
  ```
* Stop conflicting services or change ports in `config.env`
* Ensure gorchain and gorchain-rpc stacks use different ports if running on the same host

#### Issue: Container fails to start

**Solution:**
* Check container logs:
  ```bash
  docker logs $(docker ps -qf label='role=rpc')
  ```
* Verify environment variables are set correctly in `config.env`
* Ensure SSL certificate and private key paths are correct
* Check disk space availability:
  ```bash
  df -h
  ```

#### Issue: Cannot connect to validator

**Symptoms:** RPC node cannot reach validator entrypoint

**Solution:**
* Verify `KNOWN_VALIDATOR` is set correctly:
  ```bash
  echo $KNOWN_VALIDATOR
  ```
* Check `VALIDATOR_ENTRYPOINT` is correct in `config.env`
* Test network connectivity:
  ```bash
  ping gorbagana.vaasl.io
  ```
* Ensure firewall allows outbound connections on gossip port

### Network Issues

#### Issue: RPC endpoint not accessible externally

**Solution:**
* Verify `PUBLIC_RPC_ADDRESS` is set in `config.env` if external access is needed
* Check firewall rules allow inbound traffic on RPC ports:
  ```bash
  sudo ufw status
  sudo ufw allow 8899/tcp
  sudo ufw allow 8900/tcp
  ```
* Ensure your hosting provider's security groups allow these ports
* Verify the node is running:
  ```bash
  docker ps | grep rpc
  ```

#### Issue: SSL certificate errors

**Solution:**
* Verify certificate and private key files exist and are readable:
  ```bash
  ls -la ~/workspace/agave-gor-fork/origin.cert.pem
  ls -la ~/workspace/agave-gor-fork/private_key_for_origin_cert
  ```
* Check certificate validity:
  ```bash
  openssl x509 -in ~/workspace/agave-gor-fork/origin.cert.pem -text -noout
  ```
* Ensure certificate matches your domain

### Performance Issues

#### Issue: High memory usage

**Solution:**
* Monitor memory usage:
  ```bash
  docker stats
  ```
* Ensure you meet minimum hardware requirements (64GB RAM)
* Consider increasing swap space if needed
* Check for memory leaks in logs

#### Issue: Slow synchronization

**Solution:**
* Verify network connection speed meets requirements (50+ Mbps)
* Check disk I/O performance:
  ```bash
  iostat -x 1
  ```
* Ensure you're using SSD storage as recommended
* Check if validator is experiencing issues

### Monitoring Issues

#### Issue: InfluxDB not receiving data

**Solution:**
* Verify InfluxDB is running:
  ```bash
  docker ps | grep influxdb
  ```
* Check InfluxDB logs:
  ```bash
  docker logs $(docker ps -qf name=influxdb)
  ```
* Ensure the setup script was run:
  ```bash
  laconic-so --stack $stacks/gorchain-monitoring deploy setup
  ```
* Verify network connectivity between containers

## Getting Help

If you encounter an issue not covered here, or if these solutions don't resolve your problem:

* **Telegram Community:** Join the [official Telegram chat](https://t.me/gorbagana_portal) for community support
* **GitHub Issues:** Report bugs or request features on [GitHub](https://github.com/gorbagana-dev)
* **Twitter:** Follow [@Gorbagana_chain](https://x.com/Gorbagana_chain/) for updates and announcements
* **Documentation:** Check if information is missing or needs updating - see [Additional Resources](additional-resources.md) for more links

## Reporting Issues

When reporting an issue, please include:

1. **Environment details:**
   * Operating system and version
   * Docker version (`docker --version`)
   * laconic-so version (`laconic-so version`)

2. **Error messages:**
   * Full error output from logs
   * Relevant container logs

3. **Configuration:**
   * Relevant parts of `config.env` (remove sensitive information)
   * Deployment directory structure

4. **Steps to reproduce:**
   * Exact commands run
   * Sequence of events leading to the issue

**Note:** If you find missing information in the documentation or encounter an undocumented issue, please report it so we can improve the guides for everyone.
