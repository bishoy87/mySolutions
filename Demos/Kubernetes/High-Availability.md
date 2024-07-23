**we have a test environment need a kubernetes cluster installed by kubeadm so the target is make high-availabiltiy for master nodes and we will use the nginx as shown below:**
```console
sudo yum install nginx
sudo yum install nginx-mod-stream
sudo mkdir -p /etc/nginx/tcpconf.d
```
```console
sudo echo "include /etc/nginx/tcpconf.d/*;" >> /etc/nginx/nginx.conf
```

```console
cat << EOF | sudo tee /etc/nginx/tcpconf.d/kubernetes.conf
stream {
upstream kubernetes {
server 192.168.56.99:6443;
server 192.168.56.100.100:6443;
}

server {
listen 6443;
listen 443;
proxy_pass kubernetes;
}
}
EOF

```