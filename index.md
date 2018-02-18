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

## git

### remove submodule

* Delete the section referring to the submodule from the `.gitmodules` file
* Stage the changes via git add `.gitmodules`
* Delete the relevant section of the submodule from `.git/config`.
* Run `git rm --cached path_to_submodule` (no trailing slash)
* Run `rm -rf .git/modules/path_to_submodule`
* Commit the changes with `git commit -m "Removed submodu`
* Delete the now untracked submodule files `rm -rf path_to_submodule`