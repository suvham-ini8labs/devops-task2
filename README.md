# Assignment 2

### Install containerd

We need to install containerd on both master node and worker node.

Firstly we need to update our packages. `-y` is used for automatically confirm upgrading packages (dont provide user prompt).

```bash
$ sudo apt update
$ sudo apt upgrade -y
```

Install containerd

```bash
$ sudo apt install containerd -y
```

Create the `containerd` directory under `/etc`. `-p` flag is used to not throw errors if directory exists.

```bash
$ sudo mkdir -p /etc/containerd 
```

Output the default configuration of containerd and save it to `/etc/containerd/config.toml`.
`tee` command reads from standard input and outputs to standard output and to a file (if provided).

```bash
$ containerd config default | sudo tee /etc/containerd/config.toml
```

We now need to update the `SystemdCgroups = false` to `SystemdCgroup = true`. By doing this containerd will use systemd as cgroups driver.

We will use `sed` (stream editor) for this. `-i` flag denotes to edit the file in place.

```bash
$ sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
```

We now need to restart the containerd service to use the updated config file.
We also need to enable the containerd service to start on system startup.

```bash
$ sudo systemctl restart containerd
$ sudo systemctl enable containerd
```