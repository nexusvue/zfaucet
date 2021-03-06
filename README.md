# zecfaucet
Simple Zcash(ZEC) faucet built with Node.

[![Build Status](https://travis-ci.org/super3/zfaucet.svg?branch=master)](https://travis-ci.org/super3/zfaucet)
[![Coverage Status](https://coveralls.io/repos/github/super3/zfaucet/badge.svg?branch=master)](https://coveralls.io/github/super3/zfaucet?branch=master)
[![License](https://img.shields.io/badge/license-AGPLv3-blue.svg?label=license)](https://github.com/Storj/super3/zfaucet/blob/master/LICENSE)
[![GitHub contributors](https://img.shields.io/github/contributors/super3/zfaucet.svg)](https://gitHub.com//super3/zfaucet/graphs/contributors/)

## DB and Zcash Setup
#### Download and Install [RethinkDB](https://www.rethinkdb.com/)
```bash
source /etc/lsb-release && echo "deb http://download.rethinkdb.com/apt $DISTRIB_CODENAME main" | sudo tee /etc/apt/sources.list.d/rethinkdb.list
wget -qO- https://download.rethinkdb.com/apt/pubkey.gpg | sudo apt-key add -
sudo apt-get update
sudo apt-get install rethinkdb
```

#### Create Index for Timestamp
```bash
npm install -g recli
recli 'r.db("test").tableCreate("payouts")'
recli 'r.table("payouts").indexCreate("timestamp")'
```

#### Install [Zcash](https://z.cash/)
Use the [Zcash Debian binary packages](https://github.com/zcash/zcash/wiki/Debian-binary-packages) install guide. The [Zcash 1.0 User Guide](https://github.com/zcash/zcash/wiki/1.0-User-Guide) has additional information if needed. You will have to fully sync the node before you can send any payments.
```bash
sudo apt-get install apt-transport-https
wget -qO - https://apt.z.cash/zcash.asc | sudo apt-key add -
echo "deb [arch=amd64] https://apt.z.cash/ jessie main" | sudo tee /etc/apt/sources.list.d/zcash.list
sudo apt-get update && sudo apt-get install zcash
```

# Install & Run
Clone the repo.

```bash
git clone https://github.com/super3/zfaucet
cd ~/zfaucet
npm install
```

Save this under `~/zfaucet/.env`.

```bash
RPCUSER=[Zcash RPC Username]
RPCPASS=[Zcash RPC Password]
PORT=[Webserver Port]
COINHIVEPUBKEY=[Coinhive Public Key]
COINHIVEPRIVKEY=[Coinhive Private Key]
WITHDRAWTHRESHOLD=[Coinhive Hashes Needed to Withdraw]
```

Run with [PM2](http://pm2.keymetrics.io/).

```bash
npm install pm2 -g
pm2 start process.json
```

#### Code Update Script
We run this as a crontab ```*/5 * * * * ~/script.sh >> ~/zlog.log``` every 5 minutes.
```bash
#!/usr/bin/env sh
cd ~/zfaucet
git fetch && git reset --hard origin/master
```
