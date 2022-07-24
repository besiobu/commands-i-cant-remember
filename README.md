# commands-i-cant-remember
A list of commands that for some reason I just can seem to remember.

## Bash
### Create an archive
```
tar -czvf name-of-archive.tar.gz /path/to/dir
```

### Unpack archive
```
tar -xvf name-of-archive.tar.gz -C /path/to/dir
```  

### Copy files to and from remote machines
To remote
```
```
From remote
```
```

### SSH without password
Option 1:
```
ssh-keygen                 # Generate key
ssh-copy-id -i user@host   # Add key to known_hosts on host
ssh root@host              # SSH will not require password
```

Option 2:
```
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null user@host
```

### Add SSH  keep alive
Add
```
Host *
    TCPKeepAlive yes
    ServerAliveInterval 30
```
to `~/.ssh/config`.

### Enable passwordles sudo
```
echo "user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
```

### Show service log for current boot
```
journalctl -u service_name.service -f -b
```

### Generate random string (20 characters, letters only)
```
cat /dev/urandom | tr -dc '[:alpha:]' | fold -w ${1:-20} | head -n 1
```

### Check hash of file
```
sha512sum /path/to/file
```

### Binary copy of drive
```
dd bs=1M if=/dev/sdX of=/dev/sdY
```

### Erase drive
Get the drive numbers from output of `df -h` or `lsblk`.

Option 1 (zeros):
```
dd if=/dev/zero of=/dev/sdX bs=1M
```

Option 2 (random):
```
dd if=/dev/urandom of=/dev/sdX bs=1M
```

### Show memory usage by process
```
ps -eo pid,rss,cmd --sort -rss | head -10
```

### Show cpu usage by process
```
ps -eo pid,pcpu,cmd --sort -pcpu | head -10
```

### Pin all threads to cpus
Option 1:
```
cat /etc/default/grub
```
Add `isolcpus=0,1` to the lines with `GRUB_CMDLINE_LINUX=`.
```
update-grub && reboot
```

Option 2:
```
#!/bin/sh

MASK="0,1"
PIDS=$(ps -aux | awk -F ' ' '{print $2}')

for pid in $PIDS
do
    echo "Setting mask for process ${pid}."
    if taskset -cpa "${MASK}" "${pid}"
    then
        echo "Ok."
    else
        echo "Failed to set mask for ${pid}"
        exit 1
    fi
done
```

## Git
### Find "lost commits"
```
git reflog
```

### Grep commit messages
```
git log --all --grep='a regex'
```

### Grep code in commits
```
git grep 'a regex' $(git rev-list --all)
```

### Show what a file looked like at specific commit
```
git show SHA:<repo-root>/path/to/file
```

### Switch order of last two commits
Do a interactive rebase:
```
git rebase -i HEAD~2
```
Switch order:
```
pick sha2 title2
pick sha1 title1
```

### Find branch containing commit
```
git branch --contains sha 
```

## Docker
### Stop all containers
```
docker ps -aq | xargs docker stop
```

### Check how much space is used by Docker
```
docker system df
```

### Remove old images
```
docker image prune --all --force
```

### Check resource usage by container
```
docker stats --no-stream
```

## Ansible

### Basic inventory and config example
```
(.venv) dev@vm:~/ansible$ cat inventory 
[all]
vm0
vm1
vm2
```

```
(.venv) dev@vm:~/ansible$ cat inventory 
[defaults]
INVENTORY = inventory
```

```
dev@vm:~/ansible$ cat /etc/hosts
...

123.123.123.1	vm0
123.123.123.2	vm1
123.123.123.3	vm2
123.123.123.4	vm3

...
```

### Run ad hoc command as user
```
ansible example -m ping -u dev
```

### Run playbook
```
ansible-playbook -i inventory playbooks/name_of_playbook.yml
```

### Run playbook on specific host
```
ansible-playbook -i "host," playbooks/run-name_of_playbook.yml
```

## Python
### Create venv with specificy Python version
Option 1:
```
virtualenv .venv --python=python3.6
```

Option 2:
```
python3.6 -m venv .venv
```

## Virtualization
Basic vm operations:
```
virsh start vm
virsh suspend vm   # vm consumes memory but not cpu
virsh resume vm
virsh reset vm     # immediate, risk of data loss
virsh shutdown vm
virsh destroy vm   # immediate shutown, add --graceful to flush cache to disk
```

### Check IP address assigned to vms by DHCP
```
virsh net-dhcp-leases net_name 
```

```
virsh domifaddr vm0
```

### Networking
Basic network operations
```
virsh net-destroy net_name # Stop network
virsh net-edit net_name    # Edit settings
virsh net-start net_name   # Start network
```

### Clone VM
```
virt-clone --original vm --name vm-clone --file /path/to/vm/image
```

### Create snapshot of VM
Create: (requires qcow2)
```
virsh snapshot-create-as --domain vm --name "snapshot_name"
```

Revert:
```
virsh snapshot-revert vm snapshot_name
```

## Networking
### Check if DNS resolves
#### Option 1:
```
ping -c 1 google.com
```

#### Option 2:
```
dig +short google.com
```

#### Option 3:
Check Google's DNS entry on Cloudflares DNS service.
```
nslookup google.com 1.1.1.1
```

### List ports with pid's
```
sudo netstat -tulpn
```
