# Commands to run for UFW firewall on Cardano Node

- On Relay Node:
```
ufw default deny incoming
ufw default allow outgoing
ufw limit proto tcp from any to any port [custom ssh port]
ufw allow [relay port]/tcp
```

- On Block Producer Node:
```
ufw default deny incoming
ufw default allow outgoing
ufw limit proto tcp from any to any port [custom ssh port]
sudo ufw allow from [relay 1 ip] to any port [bp node port] proto tcp
sudo ufw allow from [relay 2 ip] to any port [bp node port] proto tcp
sudo ufw allow from [relay 3 ip] to any port [bp node port] proto tcp
```
