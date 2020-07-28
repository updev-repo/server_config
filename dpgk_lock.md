## SERVER ENVIRONMENT SETUP ON GOOGLE CLOUD COMPUTE ENGINE

```angular2html
lsof /var/lib/dpkg/lock
lsof /var/lib/apt/lists/lock
lsof /var/cache/apt/archives/lock

sudo kill -9 PID

sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock

sudo dpkg --configure -a
```

+ You should find out which process is holding the lock-frontend using the lsof command as discussed in previous sections:
  + `sudo lsof /var/lib/dpkg/lock-frontend`
+ This is what it showed for me:
```
     abhishek@nuc:~$ sudo lsof /var/lib/dpkg/lock-frontend
    lsof: WARNING: can't stat() fuse.gvfsd-fuse file system /run/user/1000/gvfs
      Output information may be incomplete.
    COMMAND    PID USER   FD   TYPE DEVICE SIZE/OFF   NODE NAME
    unattende 2823 root    5uW  REG    8,2        0 145221 /var/lib/dpkg/lock-frontend
```
+ next do below command
```
sudo kill -9 PID
sudo rm /var/lib/dpkg/lock-frontend
sudo apt update
```
