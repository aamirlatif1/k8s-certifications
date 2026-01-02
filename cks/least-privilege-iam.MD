## Using least-privileged identity and access management

View all user list
```bash
cat /etc/passwd
```

View groups
```bash
cat /etc/group
```

Add user as k8s operator
```bash
sudo adduser k8s-operator

info: Adding user `k8s-operator' ...
info: Selecting UID/GID from range 1000 to 59999 ...
info: Adding new group `k8s-operator' (1001) ...
info: Adding new user `k8s-operator' (1001) with group `k8s-operator (1001)' ...
info: Creating home directory `/home/k8s-operator' ...
info: Copying files from `/etc/skel' ...
New password: 
Retype new password: 
passwd: password updated successfully
Changing the user information for k8s-operator
Enter the new value, or press ENTER for the default
	Full Name []: Aamir
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] y
info: Adding new user `k8s-operator' to supplemental / extra groups `users' ...
info: Adding user `k8s-operator' to group `users' ...
```

Add group for k8s-operator
```bash
sudo groupadd k8s-ops
```
Assign group to k8s-operator
```bash
sudo usermod -aG k8s-ops k8s-operator 
```

```bash
sudo visudo -f /etc/sudoers.d/k8s-operators

%k8s-ops ALL=(ALL) NOPASSWD: /usr/bin/kubectl, /usr/bin/kubeadm

su k8s-operator 
```

Set following provilleges

```bash
sudo chmod 600 /etc/kubernetes/admin.conf
sudo chown root:root /etc/kubernetes/admin.conf
sudo chmod -R 600 /etc/kubernetes/pki
sudo chown -R root:root /etc/kubernetes/pki
```

for more fine grain control use `acl` lib
```bash
sudo apt-get install acl
sudo setfacl -R -m g:k8s-ops:rx /etc/kubernetes
sudo setfacl -R -m g:k8s-ops:rx /var/lib/kubelet
```

Audit logs

```bash
sudo apt-get install auditd
# update audit/rules.d/kubernetes.rules
-w /etc/kubernetes -p wa -k kubernetes_config 
-w /var/lib/kubelet -p wa -k kubelet_data
```
Now let's create and delet files
```bash
sudo touch /etc/kubernetes/testfile
sudo rm /etc/kubernetes/testfile 

sudo rm /var/lib/kubelet/testfile 
sudo ausearch -k kubernetes_config
```

Then you can view logs by following commands
```bash
sudo ausearch -k kubernetes_config
sudo ausearch -k kubelet_data
```