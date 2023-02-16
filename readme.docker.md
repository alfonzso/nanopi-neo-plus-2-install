## Simple docker install, don't belive the official site ! :D
```bash
sudo apt-get update
sudo apt-get install \
  ca-certificates \
  curl \
  gnupg \
  lsb-release

sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

!!! THEN !!!
Simply install:
```
sudo apt install docker.io
sudo usermod -aG docker $USER
```
and that's all you need, nothing more ...

## Strange errors in docker daemon logs (optional, don't really know its needed or not )
Docker errors tell me they need some other packages, like zfs ...

```bash
sudo apt install fuse-overlayfs
sudo apt install zfs
sudo apt install zfsutils-linux
```
