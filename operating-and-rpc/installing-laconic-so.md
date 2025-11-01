# Installation of laconic-so

Stack Orchestrator (laconic-so) is a Python3 CLI tool that allows building and deployment of a Laconic Stack on a single machine with minimal prerequisites. It runs on any OS with Python3 and Docker.

## Prerequisites

Before installing laconic-so, ensure that the following are already installed on your system:

* **Python3**: `python3 --version` >= `3.8.10` (the Python3 shipped in Ubuntu 20+ is good to go)
* **Docker**: `docker --version` >= `20.10.21`
* **jq**: `jq --version` >= `1.5`
* **git**: `git --version` >= `2.10.3`

**Note**: If installing docker-compose via package manager on Linux (as opposed to Docker Desktop), you must install the plugin:

```bash
mkdir -p ~/.docker/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.11.2/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
```

## Installation Steps

1. **Choose an installation directory**

   Decide on a directory where you would like to put the stack-orchestrator program. Typically this would be a "user" binary directory such as `~/bin` or perhaps `/usr/local/laconic` or possibly just the current working directory.

2. **Download the latest release**

   Download the latest release from the [releases page](https://git.vdb.to/cerc-io/stack-orchestrator/tags) into your chosen directory (we're using `~/bin` below for concreteness but edit to suit if you selected a different directory). Also be sure that the destination directory exists and is writable:

   ```bash
   curl -L -o ~/bin/laconic-so https://git.vdb.to/cerc-io/stack-orchestrator/releases/download/latest/laconic-so
   ```

3. **Set execute permissions**

   ```bash
   chmod +x ~/bin/laconic-so
   ```

4. **Ensure laconic-so is on your PATH**

   Make sure the directory containing `laconic-so` is in your system's PATH environment variable.

5. **Verify installation**

   Verify operation (your version will probably be different, just check here that you see some version output and not an error):

   ```bash
   laconic-so version
   ```

   You should see output similar to:
   ```
   Version: 1.1.0-7a607c2-202304260513
   ```

6. **Save the distribution URL**

   Save the distribution url to `~/.laconic-so/config.yml`:

   ```bash
   mkdir ~/.laconic-so
   echo "distribution-url: https://git.vdb.to/cerc-io/stack-orchestrator/releases/download/latest/laconic-so" > ~/.laconic-so/config.yml
   ```

## Updating laconic-so

If Stack Orchestrator was installed using the process described above, it is able to subsequently self-update to the current latest version by running:

```bash
laconic-so update
```

## Quick Install Script

**To get started quickly** on a fresh Ubuntu instance (e.g., Digital Ocean), you can use the [quick install script](https://github.com/cerc-io/stack-orchestrator/blob/main/scripts/quick-install-linux.sh). **WARNING:** always review scripts prior to running them so that you know what is happening on your machine.

For more information, visit the [Stack Orchestrator GitHub repository](https://github.com/cerc-io/stack-orchestrator).

