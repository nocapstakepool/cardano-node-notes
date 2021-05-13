FIRST: Take an Backup/Snapshot from your Nodes!!!

– Download new binaries –
```
cd $HOME/git
rm -rf cardano-node-old/
git clone https://github.com/input-output-hk/cardano-node.git cardano-node2
cd cardano-node2/
```

– Checkout new Branch 1.27.0 and Compile –
```
cd $HOME/git/cardano-node2
cabal update
git fetch --all --recurse-submodules --tags
git checkout tags/1.27.0
cabal configure -O0 -w ghc-8.10.4
echo -e "package cardano-crypto-praos\n flags: -external-libsodium-vrf" > cabal.project.local
cabal build cardano-node cardano-cli
```
Note: Depending on your server performance, this process can take between 20 and 60 minutes.

– Check if cardano-cli and cardano-node updated to 1.27.0 –
```
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") version
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") version
```
If it print Versions 1.27.0 than all is fine

– Stop old Cardano Node 1.26.2 and replace Binaries with the new Version – Stop Node:
```
sudo systemctl stop cardano-node
```

- Copy Binaries to bin folder:
```
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") /usr/local/bin/cardano-cli
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") /usr/local/bin/cardano-node
```

- Verify Version:
```
cardano-node version
cardano-cli version
```

- If versions 1.27.0 you can start your node:
```
sudo systemctl start cardano-node
Check using ./gLiveView.sh
```

– Clean up old directories
```
cd $HOME/git
mv cardano-node/ cardano-node-old/
mv cardano-node2/ cardano-node/
```
