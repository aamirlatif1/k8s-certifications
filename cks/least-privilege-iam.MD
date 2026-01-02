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