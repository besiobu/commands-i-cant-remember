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
