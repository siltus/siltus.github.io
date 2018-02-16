# My cool scripts

## SSH

### Mount SSH as folder

```
sshfs name@server:/path/to/folder /path/to/mount/point
```

### Remove a host from known_hosts (when machine signature changes)

```
ssh-keygen -R <the_offending_host>
```

## Linux

### Count how many traffic goes through localhost:8888
```
sudo iftop -i lo -P -n -f 'dst port 8888 || src port 8888' 
```

### rsync

// TODO

### screen

// TODO

