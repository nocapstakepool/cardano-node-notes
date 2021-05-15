**Create swap file and enable:**
```
sudo fallocate -l 8G /swapram
sudo chmod 600 /swapram
sudo mkswap /swapram
sudo swapon /swapram
echo '/swapram none swap sw 0 0' | sudo tee -a /etc/fstab
```

**Check that it worked.**
```
sudo swapon --show
```

**Increase swappiness of swap by editing the sysctl.conf:**
```
sudo nano /etc/sysctl.conf
```

**Adding lines below to bottom of sysctl.conf file**
```
vm.swappiness=10
vm.vfs_cache_pressure=50
```

**Reboot**
```
sudo reboot
