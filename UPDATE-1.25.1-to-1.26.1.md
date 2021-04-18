# cardano-node-notes
Notes

Thank you anti-biz:
https://forum.cardano.org/t/steps-for-1-26-1-non-cntools-coincashew-users/56692/40

To update with $HOME/git/cardano-node as the current binaries directory, copy the whole cardano-node directory to a new place so that you have a backup. Also make sure to do a snapshot of your server/db if anything goes wrong.
```
sudo apt-get update
sudo apt-get upgrade
sudo reboot
```

```
cd $HOME/git
mv cardano-node cardano-node-old
git clone https://github.com/input-output-hk/cardano-node.git cardano-node2
cd cardano-node2/
```

Read the patch notes for any other special updates or dependencies that may be required for the latest release.

Remove the old binaries and rebuild the latest binaries. Run the following command to pull and build the latest binaries. Change the checkout tag or branch as needed.
```
cd $HOME/git/cardano-node2
git clean -fd
git fetch --all --recurse-submodules --tags
git checkout tags/1.26.1 && git pull
```

Install ghcup (optional - if not yet installed)
```
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
source ~/.bashrc
```

OR 
```
Upgrade ghcup
```

```
ghcup upgrade  (just to make sure ghcup is on the latest version)
ghcup install ghc 8.10.4
ghcup set ghc 8.10.4
ghc --version   (just to check the correct version)
ghcup install cabal 3.4.0.0
ghcup set cabal 3.4.0.0
cabal --version   (just to check the correct version)
cabal update  (just to make sure all dependencies are in the info)
```
GHCUP changes the paths (Cabal 3.4.0.0 not changing from 3.2.0.0 (solved) - #2 by Anti.biz) which cabal / which ghc`

```
cabal configure -O0 -w ghc-8.10.4
echo -e "package cardano-crypto-praos\n flags: -external-libsodium-vrf" > cabal.project.local
cabal build cardano-node cardano-cli
```

Verify your cardano-cli and cardano-node were updated to the expected version.
```
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") version
$(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") version
```

Stop your node before updating the binaries.
```
sudo systemctl stop cardano-node
```

Copy cardano-cli and cardano-node files into bin directory.
```
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-cli") /usr/local/bin/cardano-cli
sudo cp $(find $HOME/git/cardano-node2/dist-newstyle/build -type f -name "cardano-node") /usr/local/bin/cardano-node
```

Verify your cardano-cli and cardano-node were copied successfully and updated to the expected version.
```
cardano-node version
cardano-cli version
```

Update the NODE_BULD_NUM
```
export NODE_BUILD_NUM=$(curl https://hydra.iohk.io/job/Cardano/iohk-nix/cardano-deployment/latest-finished/download/1/index.html | grep -e "build" | sed 's/.*build\/\([0-9]*\)\/download.*/\1/g')
sed -i $HOME/.bashrc \
    -e "s/export NODE_BUILD_NUM=[0-9]\+/export NODE_BUILD_NUM=${NODE_BUILD_NUM}/g"
source $HOME/.bashrc
sudo systemctl start cardano-node
```

```
cd $HOME/git
mv cardano-node/ cardano-node-old/
mv cardano-node2/ cardano-node/
```

Do gliveView updates now (if any)

Check tip
```
cardano-cli query tip --mainnet
```

In “topologyUpdater.sh” change the script to query the tip from blockNo to block line 21
```
blockNo=$(/usr/local/bin/cardano-cli query tip ${NETWORK_IDENTIFIER} | jq -r .block )
```
