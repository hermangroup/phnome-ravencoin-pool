Crypticwizardry Ravencoin Pool - v1.0 Baseline Edition
================

[![License](https://img.shields.io/badge/license-GPL--3.0-blue)](https://opensource.org/licenses/GPL-3.0)

Highly Efficient Mining Pool for Ravencoin!

-------
### Screenshots
#### Home<br />
![Home](https://raw.githubusercontent.com/Racing1/phoenixmax-ravencoin-pool/master/docs/frontend/home.png)

-------
### Node Open Mining Portal consists of 3 main modules:
| Project | Link |
| ------------- | ------------- |
| [Crypticwizardry Ravencoin Pool](https://github.com/rvnminers-A-and-N/crypticwizardry-ravencoin-pool) | https://github.com/rvnminers-A-and-N/crypticwizardry-ravencoin-pool |
| [Crypticwizardry Ravencoin Stratum](https://github.com/rvnminers-A-and-N/kawpow-stratum-pool) | https://github.com/rvnminers-A-and-N/kawpow-stratum-pool |
| [Crypticwizardry Ravencoin Hashing](https://github.com/rvnminers-A-and-N/crypticwizardry-ravencoin-hashing) | https://github.com/rvnminers-A-and-N/crypticwizardry-ravencoin-hashing |

-------
### Requirements
***NOTE:*** _These requirements will be installed in the install section!_<br />
* Ubuntu Server 18.04.* LTS
* Coin daemon
* Node Version Manager
* Node 8.1.7
* Process Manager 2 / pm2
* Redis Server
* ntp

-------

### Install RavenCoin Daemon

    adduser pool
    usermod -aG sudo pool
    su - pool
    sudo apt install wget
    wget https://github.com/RavenProject/Ravencoin/releases/download/v4.3.2.1/raven-4.3.2.1-x86_64-linux-gnu.zip
    unzip raven-4.3.2.1-x86_64-linux-gnu.zip
    rm raven*zip // this step is optional //
    cd linux
    tar -xvf raven-4.3.2.1-x86_64-linux-gnu.tar.gz
    rm raven*gz // this step is optional //
    cd raven-4.3.2.1/bin
    mkdir -p ~/.raven/
    touch ~/.raven/raven.conf
    echo "rpcuser=user1" > ~/.raven/raven.conf
    echo "rpcpassword=pass1" >> ~/.raven/raven.conf
    echo "prune=550" >> ~/.raven/raven.conf
    echo "daemon=1" >> ~/.raven/raven.conf
    ./ravend
    ./raven-cli getnewaddress

Example output: RNs3ne88DoNEnXFTqUrj6zrYejeQpcj4jk - it is the address of your pool, you need to remember it and specify it in the configuration file pool_configs/ravencoin.json.
    
    ./raven-cli getaddressesbyaccount ""
    
Information about pool wallet address.
    
    ./raven-cli getwalletinfo
    
Get more information.

    ./raven-cli getblockcount
    
Information about synchronization of blocks in the main chain.

    ./raven-cli help
Other helpfull commands.

-------

### Install Pool

    sudo apt install git -y
    cd ~
    git config --global http.https://gopkg.in.followRedirects true
    git clone https://github.com/rvnminers-A-and-N/crypticwizardry-ravencoin-pool.git
    cd crypticwizardry-ravencoin-pool/
    ./install.sh

-------
### Configure Pool

Change "stratumHost": "crypticwizardry.com", to your IP or DNS in file config.json:

    cd ~/crypticwizardry-ravencoin-pool
    nano config.json

```javascript
{
    "poolname": "Ravencoin Pool",
    "devmode": false,
    "devmodePayMinimim": 0.25,
    "devmodePayInterval": 120,
    "logips": true,       
    "anonymizeips": true,
    "ipv4bits": 16,
    "ipv6bits": 16,
    "defaultCoin": "ravencoin",
    "poollogo": "/website/static/img/poollogo.png",
    "discord": "Join our #mining channel on Discord: ",
    "discordlink": "https://discord.gg/",
    "pagetitle": "Ravencoin Pool - 2.0 % Fees Promo - Run By Professionals",
    "pageauthor": "JAMPS,rvnminers-A-and-N",
    "pagedesc": "A reliable, low fee, easy to use mining pool for Ravencoin! Get started mining today!",
    "pagekeywds": "GPU,CPU,Hash,Hashrate,Cryptocurrency,Crypto,Mining,Pool,Ravencoin,Easy,Simple,How,To",
    "btcdonations": "Add_Here",
    "ltcdonations": "Add_Here",
    "rvndonations": "Add_Here",
    "logger" : {
        "level" : "debug",
        "file" : "/home/pool/logs-ravencoin-pool/logs/ravencoin_debug.log"
    },
    "cliHost": "127.0.0.1",
    "cliPort": 17117,
    "clustering": {
        "enabled": false,
        "forks": "auto"
    },
    "defaultPoolConfigs": {
        "blockRefreshInterval": 400,
        "jobRebroadcastTimeout": 25,
        "connectionTimeout": 600,
        "emitInvalidBlockHashes": false,
        "validateWorkerUsername": true,
        "tcpProxyProtocol": false,
        "banning": {
            "enabled": true,
            "time": 600,
            "invalidPercent": 50,
            "checkThreshold": 500,
            "purgeInterval": 300
        },
        "redis": {
            "host": "127.0.0.1",
            "port": 6379
        }
    },
    "website": {
        "enabled": true,
        "sslenabled": false,
        "sslforced": false,
        "host": "0.0.0.0",
        "port": 80,
        "sslport": 443,
        "sslkey": "/home/pool/logs-ravencoin-pool/certs/privkey.pem",
        "sslcert": "/home/pool/logs-ravencoin-pool/certs/fullchain.pem",
        "stratumHost": "crypticwizardry.com",
        "stats": {
            "updateInterval": 30,
            "historicalRetention": 43200,
            "hashrateWindow": 900
        }
    },
    "redis": {
        "host": "127.0.0.1",
        "port": 6379
    }
}

```

Change "address": "your_pool_node_wallet_address", to your pool created wallet address in file ravencoin.json:

    cd ~/crypticwizardry-ravencoin-pool/pools
    nano ravencoin.json

```javascript
{
    "enabled": true,
    "coin": "ravencoin.json",
    "address": "your_pool_node_wallet_address",
    "donateaddress": "your_personal_wallet_address",
    "rewardRecipients": {
        "your_personal_wallet_address": 2.0 // pool fee //
    },
    "paymentProcessing": {
        "enabled": true,
        "schema": "PROP",
        "paymentInterval": 300, // 5 minute payment interval - change to your liking //
        "minimumPayment": 10, // minimunm payout set here //
        "maxBlocksPerPayment": 1,
        "minConf": 30,
        "coinPrecision": 8,
        "daemon": {
            "host": "127.0.0.1",
            "port": 8766,
            "user": "user1",
            "password": "pass1"
        }
    },
    "ports": {
	"10008": {
            "diff": 0.05,
    	    "varDiff": {
    	        "minDiff": 0.025,
    	        "maxDiff": 1024,
    	        "targetTime": 10,
    	        "retargetTime": 60,
    	        "variancePercent": 30,
    		"maxJump": 25
    	    }
        },
        "10016": {
	    "diff": 0.10,
            "varDiff": {
                "minDiff": 0.05,
                "maxDiff": 1024,
    	        "targetTime": 10,
    	        "retargetTime": 60,
    	        "variancePercent": 30,
		"maxJump": 25
            }
        },
        "10032": {
	    "diff": 0.20,
            "varDiff": {
    		"minDiff": 0.10,
    		"maxDiff": 1024,
    	        "targetTime": 10,
    	        "retargetTime": 60,
    	        "variancePercent": 30,
    		"maxJump": 50
    	    }
        },
	"10256": {
	    "diff": 1024000000,
            "varDiff": {
                "minDiff": 1024000000,
                "maxDiff": 20480000000,
    	        "targetTime": 10,
    	        "retargetTime": 60,
    	        "variancePercent": 30,
		"maxJump": 25
            }
        }
    },
    "daemons": [
        {
            "host": "127.0.0.1",
            "port": 8766,
            "user": "user1",
            "password": "pass1"
        }
    ],
    "p2p": {
        "enabled": false,
        "host": "127.0.0.1",
        "port": 8767,
        "disableTransactions": true
    },
    "mposMode": {
        "enabled": false,
        "host": "127.0.0.1",
        "port": 3306,
        "user": "me",
        "password": "mypass",
        "database": "rvn",
        "checkPassword": true,
        "autoCreateWorker": false
    }
}

```

### Run Pool
    
    cd ~/crypticwizardry-ravencoin-pool
    sudo bash pool-start.sh

### Donations for developers of Crypticwizardry Ravencoin Pool

BTC: (Will Add)

ETH: (Will Add)

LTC: (Will Add)

RVN: (Will Add)
    
-------

## License
```
Licensed under the GPL-3.0
Copyright (c) 2021 phoenixmax.org
```
