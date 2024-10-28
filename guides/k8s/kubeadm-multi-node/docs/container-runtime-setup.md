# Container Runtime Setup

Install and configure `containerd` as the container runtime.

## Install runc

Download and install `runc`:

```bash
wget https://github.com/opencontainers/runc/releases/download/v1.1.12/runc.amd64
sudo install -m 755 runc.amd64 /usr/local/sbin/runc
```

---

## Install CNI Plugins

Download and install the CNI plugins:

```bash
wget https://github.com/containernetworking/plugins/releases/download/v1.4.1/cni-plugins-linux-amd64-v1.4.1.tgz
sudo mkdir -p /opt/cni/bin
sudo tar -C /opt/cni/bin -xzvf cni-plugins-linux-amd64-v1.4.1.tgz
```

---

## Install containerd

Download and install `containerd`:

```bash
wget https://github.com/containerd/containerd/releases/download/v1.7.6/containerd-1.7.6-linux-amd64.tar.gz
sudo tar -C /usr/local -xzvf containerd-1.7.6-linux-amd64.tar.gz
```

Set up the systemd service:

```bash
sudo mkdir -p /usr/local/lib/systemd/system

wget https://raw.githubusercontent.com/containerd/containerd/main/containerd.service
sudo mv containerd.service /usr/local/lib/systemd/system/

sudo systemctl daemon-reload
sudo systemctl enable --now containerd
```

---

## Configure containerd

Generate the default configuration file:

```bash
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
```

Edit `/etc/containerd/config.toml` to use `systemd` as the cgroup driver:

```bash
sudo nano /etc/containerd/config.toml
```

Set `SystemdCgroup = true` under the `runc.options` section:

```toml
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  ...
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
```

Restart `containerd`:

```bash
sudo systemctl restart containerd
```
