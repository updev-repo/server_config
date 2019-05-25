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