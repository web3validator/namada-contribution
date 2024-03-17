
This guide outlines how to update the address book and genesis file for a node, how to download and apply a new snapshot for system updates, including backup steps and clean-up procedures.

## Address Book Update
Refresh the addrbook hourly:

```bash
wget -O $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/config/addrbook.json https://testnet-files.itrocket.net/namada/addrbook.json
```
## Genesis File 
Fetch the latest genesis file:
```bash
wget -O $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/config/genesis.json https://testnet-files.itrocket.net/namada/genesis.json
```

## Snapshot Service
Snapshot details - height: 159347, updated every 4 hours, size: 14GB. Download and apply the snapshot with the following commands:

```bash
cd $HOME
wget -O snap_namada.tar https://testnet-files.itrocket.net/namada/snap_namada.tar
# Stop the node
sudo systemctl stop namadad
# Backup and prepare for the new snapshot
cp $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/data/priv_validator_state.json $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/priv_validator_state.json.backup
rm -rf $HOME/.local/share/namada/shielded-expedition.88f17d1d14/db $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/data
tar -xvf $HOME/snap_namada.tar -C $HOME/.local/share/namada/shielded-expedition.88f17d1d14
mv $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/priv_validator_state.json.backup $HOME/.local/share/namada/shielded-expedition.88f17d1d14/cometbft/data/priv_validator_state.json
# Restart the node
sudo systemctl restart namadad && sudo journalctl -u namadad -f
# Clean up
rm -rf $HOME/snap_namada.tar
```
