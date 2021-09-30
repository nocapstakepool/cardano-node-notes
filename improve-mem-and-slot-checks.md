# Improve Memory and Missed Leadership Slot Checks 

### Ubuntu-server Node Missed Leadership Slot Checks improvements

1. Edit ~/.bashrc then `source ~/.bashrc`
```
export GHCRTS='-N -T -I0 -A16m --disable-delayed-os-memory-return --nonmoving-gc'
```

2. Edit `cardano-node run` config
```
cardano-node +RTS -N -RTS run
```

3. Restart your Cardano node:
```
sudo systemctl restart cardano-node
```

## PI Memory Usage improvements

1. `Edit cardano-node run` config
```
+RTS -N4 --disable-delayed-os-memory-return -I0.3 -Iw300 -A16m -n4m -F1.5 -H3G -T -S -RTS
```

2. Restart your Cardano node:
```
sudo systemctl restart cardano-node
```
