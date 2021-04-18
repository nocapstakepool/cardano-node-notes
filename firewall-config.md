# Commands to run for UFW firewall on Cardano Node

- Firewall Status
```
sudo ufw status
```

- Delete Firewall Rule
```
sudo ufw status numbered
sudo ufw delete [number of rule]
```

- On Relay Node:
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw limit proto tcp from any to any port [custom ssh port]
sudo ufw allow [relay port]/tcp
```

- On Block Producer Node:
```
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw limit proto tcp from any to any port [custom ssh port]
sudo ufw allow from [relay 1 ip] to any port [bp node port] proto tcp
sudo ufw allow from [relay 2 ip] to any port [bp node port] proto tcp
sudo ufw allow from [relay 3 ip] to any port [bp node port] proto tcp
```
