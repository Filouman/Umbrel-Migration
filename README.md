# Umbrel Migration

Migration guide to transfer LND from one Umbrel machine to another

***Save channel backup before you begin!***

### Old Node

*Optional:* Limit forwarding on old node to reduce risk of force-closures during migration:
```shell
bos limit-forwarding --disable-forwards
```
Wait for any pending HTLCs to clear before proceeding with migration. 
 
Open SSH session and tail LND logs to ensure it stops:
```shell
docker logs -f lightning_lnd_1
```
-or-
```shell
tail -f  ~/umbrel/app-data/lightning/data/lnd/logs/bitcoin/mainnet/lnd.log
```

Open second SSH session and stop LND:
```shell
sudo ./umbrel/scripts/stop
```
Check LND is stopped in logs in first SSH session.


### New node

Copy LND from old node to new node:
```shell
sudo rsync -arhvP --append-verify umbrel@umbrel.local:~/umbrel/app-data/lightning ~/umbrel/app-data
```
Wait for copying to finish.


### Old Node

Destroy the heart of old node (rename channel.db / lnd.conf files):
```shell
sudo mv ~/umbrel/app-data/lightning/data/lnd/data/graph/mainnet/channel.db ~/umbrel/app-data/lightning/data/lnd/data/graph/mainnet/channel.bak
sudo mv ~/umbrel/app-data/lightning/data/lnd/lnd.conf ~/umbrel/app-data/lightning/data/lnd/lnd.bak
sudo mv ~/umbrel/app-data/lightning/data/lnd/umbrel-lnd.conf ~/umbrel/app-data/lightning/data/lnd/umbrel-lnd.bak
```


### New Node

Start LND in first terminal session:
```shell
sudo ./umbrel/scripts/start
```
Open a second SSH session and tail LND logs to ensure it starts completely with no errors:
```shell
docker logs -f lightning_lnd_1
```
-or-
```shell
tail -f  ~/umbrel/app-data/lightning/data/lnd/logs/bitcoin/mainnet/lnd.log
```

### IMPORTANT

***Never start LND on the old node again.***
