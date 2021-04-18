# UPDATE from 1.26.1 to 1.26.2
# Important for Block Producer Nodes

Thank you saulgodman
https://forum.cardano.org/t/cardano-node-update-from-v-1-26-1-to-v-1-26-2-based-for-coincashew-user/57935

An important update for all SPOs who don’t want to miss potential blocks!

Fixed:
Stake pools unnecessarily evaluate the stake distribution at the epoch boundary

Read more about it: Github
https://github.com/input-output-hk/cardano-node/releases/tag/1.26.2
It is also sufficient to install the update only on the blockprudcer. The relays do not necessarily need it. However, it also makes sense to update relays and other airgapped machines.

FIRST:
Take an Backup/Snapshot from your Nodes!!!

– Download new binaries –
```
cd $HOME/git
rm -rf cardano-node-old/
git clone https://github.com/input-output-hk/cardano-node.git cardano-node2
cd cardano-node2/
```

– Checkout new Branch 1.26.2 and Compile –
```
cd $HOME/git/cardano-node2
cabal update
git fetch --all --recurse-submodules --tags
git checkout tags/1.26.2
cabal configure -O0 -w ghc-8.10.4
echo -e "package cardano-crypto-praos\n flags: -external-libsodium-vrf" > cabal.project.local
cabal build cardano-node cardano-cli
Depending on the server performance, the process takes between 20 and 60 minutes.
```

– Check if cardano-cli and cardano-node updated to 1.26.2 –
```
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") version
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") version
```
If it print you Version 1.26.2 than all is fine

– Stop old Cardano Node 1.26.1 and replace Binaries with the new Version –
Stop Node:
```
sudo systemctl stop cardano-node
```

Copy Binaries to bin folder:
```
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") /usr/local/bin/cardano-cli
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") /usr/local/bin/cardano-node
```

Verify Version:
```
cardano-node version
cardano-cli version
```
If Version 1.26.2 you can start your node!

```
sudo systemctl start cardano-node
```
Check using ./gLiveView.sh

– Now just clean up a little bit –
```
cd $HOME/git
mv cardano-node/ cardano-node-old/
mv cardano-node2/ cardano-node/
```
Guild LiveView:
Guild LiveView is not yet available for version 1.26.2 when you start it you will be asked if you want to update. Please enter “no” and confirm with return. There will be an official update of Guild LiveView in the next days.
