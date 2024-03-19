
This guide outlines how to update the address book and genesis file for a node, how to download and apply a new snapshot for system updates, including backup steps and clean-up procedures.

## Address Book Update
We refresh the addrbook every 4 hours.

```bash
wget -O $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/config/addrbook.json https://snapshots.posthuman.digital/namada/addrbook.json
```
## Genesis File 
Fetch the latest genesis file:
```bash
wget -O $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/config/genesis.json https://snapshots.posthuman.digital/namada/genesis.json
```

## Snapshot Service
Updated every 4 hours, size: ~ 20GB. Download and apply the snapshot with the following commands:

```bash
cd $HOME
wget -O data.tar.gz https://testnet-files.posthuman.digital/namada/data.tar.gz
# Stop the node
sudo systemctl stop namadad
# Backup and prepare for the new snapshot
cp $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/data/priv_validator_state.json $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/priv_validator_state.json.backup
rm -rf $HOME/.local/share/namada/shielded-expedition.88f17d1d14/db $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/data
tar -xvf $HOME/data.tar.gz -C $HOME/.local/share/namada/shielded-expedition.88f17d1d14
mv $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/priv_validator_state.json.backup $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/data/priv_validator_state.json
# Restart the node
sudo systemctl restart namadad && sudo journalctl -u namadad -f
# Clean up
rm -rf $HOME/data.tar.gz
```

