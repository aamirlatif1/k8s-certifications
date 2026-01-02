## Config Linux kernel setting for k8s networking
Add these lines in `/etc/sysctl.d/99-kubernetes-cri.conf` to configure networking
```bash
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv6.ip_forward = 1
```

## Install a Container Runtime

Follow steps given here https://kubernetes.io/docs/setup/production-environment/container-runtimes/