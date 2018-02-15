# My cool scripts

## Linux

### Count how many traffic goes through localhost:8888
```
sudo iftop -i lo -P -n -f 'dst port 8888 || src port 8888' 
```
