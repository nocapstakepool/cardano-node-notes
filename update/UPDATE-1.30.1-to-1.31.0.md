1. Update and restart your instance:
```
sudo apt-get update && sudo apt-get upgrade -y && sudo reboot
```

2. Download latest cardano-node git and checkout latest branch
```
cd $HOME/git
git clone https://github.com/input-output-hk/cardano-node.git cardano-node2
cd cardano-node2/
git fetch --all --recurse-submodules --tags
git checkout tags/1.31.0
```

*Note: make sure you to have at least ghc 8.10.4 and cabal 3.4.0 before proceeding.*
(There is a newer version of cabal 3.6.2.0 but I'm not sure if it's required for this upgrade - https://github.com/input-output-hk/cardano-node/blob/master/doc/getting-started/install.md - as I have not tested compiling with newer version, I suggest to stick with 3.4.0 for this build.)
```
ghcup upgrade
ghcup install ghc 8.10.4
ghcup set ghc 8.10.4
ghcup install cabal 3.4.0.0
ghcup set cabal 3.4.0.0
cabal update
ghc --version
cabal --version
```

3. Build the node.
```
cd $HOME/git/cardano-node2
cabal configure -O0 -w ghc-8.10.4
echo -e "package cardano-crypto-praos\n flags: -external-libsodium-vrf" > cabal.project.local
cabal build cardano-node cardano-cli
```

4. Check `cardano-cli` and `cardano-node` that the build was successful
```
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") version
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") version
```

5. If on 1.31.0, shut down the node and move the binaries to your bin
```
sudo systemctl stop cardano-node
```
```
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") /usr/local/bin/cardano-cli
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") /usr/local/bin/cardano-node
```

6. Check that successful version upgrades then start back up Cardano node
```
cardano-node version
cardano-cli version
```
```
sudo systemctl start cardano-node
```

7. Clean up
```
cd $HOME/git/
rm -rf cardano-node-old
mv cardano-node cardano-node-old
mv cardano-node2 cardano-node
```

Monitor the progress by either using gliveview or journalctl
```
journalctl --unit=cardano-node --follow 
```
