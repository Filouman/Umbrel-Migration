# Umbrel Migration


### OLD NODE

***Save channel backup before you begin***

Open SSH session and tail LND logs to ensure it stops:
```shell
docker logs -f lightning_lnd_1
```

Open second SSH session and stop LND:
```shell
sudo ./umbrel/scripts/stop
```

Check LND is stopped in logs in first SSH session.


### New node

Copy LND from old node to new node
```shell
sudo rsync -arhvP --append-verify umbrel@umbrel.local:~/umbrel/app-data/lightning/data/lnd ~/umbrel/app-data/lightning/data
```
Wait for copying to finish.


### Old node

Destroy the heart of old node (rename channel.db / lnd.conf files):
```shell
sudo mv ~/umbrel/app-data/lightning/data/lnd/data/graph/mainnet/channel.db ~/umbrel/app-data/lightning/data/lnd/data/graph/mainnet/channel.bak
sudo mv ~/umbrel/app-data/lightning/data/lnd/lnd.conf ~/umbrel/app-data/lightning/data/lnd/lnd.bak
sudo mv ~/umbrel/app-data/lightning/data/lnd/umbrel-lnd.conf ~/umbrel/app-data/lightning/data/lnd/umbrel-lnd.bak
```

### NEW NODE

Start LND in first terminal session:
```shell
sudo ./umbrel/scripts/start
```
Open a second SSH session and tail LND logs to ensure it starts completely with no errors:
```shell
docker logs -f lightning_lnd_1
```
