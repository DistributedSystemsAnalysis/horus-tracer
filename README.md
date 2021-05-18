# Falcon Tracer
Falcon Tracer is an eBPF-based tool for tracing a tree of processes in order to collect events for further processing in the Falcon's pipeline.

This tracer instruments specific kernel functions (using *kprobes*) for gathering the revelant data. It mainly collects events related to socket connections (connect, accept, send, receive) and process events (start, end, fork, join).

## Requirements
- Kernel 4.8+
- Python 3 (with pip support)
- [BCC tools](https://github.com/iovisor/bcc/blob/master/INSTALL.md)

## Installation
The installation process requires `sudo` permissions.

- [Install BCC tools](https://github.com/iovisor/bcc/blob/master/INSTALL.md)
- Run `sudo pip install -e .` to install it locally and to make `falcon-tracer` available globally.

The recommended environment is Ubuntu 18.04 LTS (Bionic Beaver) with kernel 5.4.0 where Falcon Tracer
can be installed with the following sequence of commands:

	# Install BCC tools (iovisor packages)	
	sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4052245BD4284CDD
	echo "deb https://repo.iovisor.org/apt/$(lsb_release -cs) $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/iovisor.list
	sudo apt-get update
	sudo apt-get -y install bcc-tools libbcc-examples linux-headers-$(uname -r)

	# Install dependencies and falcon-tracer
	sudo apt-get -y install python3-pip python3-confluent-kafka python3-bcc
	sudo pip3 install -e .

## Usage

Make sure the current working directory is `./falcon`.

### **Trace a new process by command.**

To start tracing a new process and the subsequent process tree, we recommend to use this approach.

```bash
sudo falcon-tracer command

# Example
sudo falcon-tracer wget http://google.pt
```

### **Trace a currently running process.**

Sometimes, you want to trace the behavior of a already running process. However, please note that the process tree of the running process will not be traced.

```bash
sudo falcon-tracer --pid pid

# Example
sudo falcon-tracer --pid 123
```

---

See all the available options by running `falcon-tracer --help` in your terminal.
