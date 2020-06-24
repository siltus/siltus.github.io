# My cool scripts

## SSH

### Tunnel from internet to localhost (like ngrok)

For example, if you want that server.com:1234 will reach localhost port 5678:

```
ssh -RN 5678:localhost:1234 user@server.com
```

you can also add `-f` flag so no real session will be holding the stdout

#### Forwarding SSH 

Need to edit `/etc/ssh/sshd_config` and make these settings to be more secure

```
PasswordAuthentication no
```

#### Keep alive

Install `autossh` and run the command:

```
autossh -M 0 -o "ServerAliveInterval 30" -R 5678:localhost:1234 user@server.com
```

### Mount SSH as folder

```
sshfs name@server:/path/to/folder /path/to/mount/point
```

### Remove a host from known_hosts (when machine signature changes)

```
ssh-keygen -R <the_offending_host>
```

## Networking (POSIX)

### Forward local port 12345 to local port 54321
(Useful when port 54321 is bound only to localhost and can't be accessed from internet)

```
socat TCP-LISTEN:12345,fork TCP:localhost:54321
```

## OSX

### Who listens on port each port

```
lsof -n -i -P | grep LISTEN
```

## Linux

### Get public IP of the current machine from terminal

```
dig +short myip.opendns.com @resolver1.opendns.com
```

### Count how many traffic goes through localhost:8888
```
sudo iftop -i lo -P -n -f 'dst port 8888 || src port 8888' 
```

### Listen on port 12345 (for telnet testing)
```
nc -l 12345
```

### Who listens on which port
```
sudo netstat -peanut
```

### Open port 12345 (TCP) in iptables

```
sudo iptables -A INPUT -p tcp -m state –state NEW -m tcp –dport 12345 -j ACCEPT 
```

to verify, run:

```
sudo iptables --list
```

### show full path in ps (not truncated)
```
ps aux | less -+S
```

### rsync

// TODO

### screen

// TODO

## git

### Find all changes to a specific file across all branches, with the actual diff

```
git log --all -p --source -- <path/to/file>
```

### remove submodule

* Delete the section referring to the submodule from the `.gitmodules` file
* Stage the changes via git add `.gitmodules`
* Delete the relevant section of the submodule from `.git/config`.
* Run `git rm --cached path_to_submodule` (no trailing slash)
* Run `rm -rf .git/modules/path_to_submodule`
* Commit the changes with `git commit -m "Removed submodu`
* Delete the now untracked submodule files `rm -rf path_to_submodule`

## SSL and certificates

### Display certificate data in the command line

```
echo | openssl s_client -showcerts -servername gnupg.org -connect google.com:443 2>/dev/null | openssl x509 -inform pem -noout -text
```

### Convert P12 (PKCS) file to seperate PEM key and cert

```
openssl pkcs12 -in client.p12 -out client.crt.pem -clcerts -nokeys
openssl pkcs12 -in client.p12 -out client.key.pem -nocerts -nodes
```

## Other Bash stuff

### Count files recursively by extension

```
find . -type f | sed 's/.*\.//' | sort | uniq -c
```

## jq

### filter based on field

```
jq -r 'select(.field == value)'
jq -r 'select(.field1 != .field2)'
```

### create new object based on piped objects

```
jq -r '{ a: .f1, b: .f2.f3 }'
```

## Wireshark / Tshark

### Print all packets body that satisfy a condition
```
tshark -r file.pcap -Y 'http && tcp contains "blabla"' -T fields -e http.file_data
```

## AWS

### get supported availability zone for a specific machine type
```
aws ec2 describe-reserved-instances-offerings --filters 'Name=scope,Values=Availability Zone' --no-include-marketplace --instance-type $TYPE | jq -r '.ReservedInstancesOfferings[].AvailabilityZone' | sort | uniq
```

Where TYPE is "r5.large" etc.
