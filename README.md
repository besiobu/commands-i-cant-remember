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

### Enable passwordles sudo
```
user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
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
