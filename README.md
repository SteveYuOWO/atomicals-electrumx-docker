# Atomicals ElectrumX Docker

Aim to provide a simple and easy way to run [atomicals-electrumx](https://github.com/atomicals/atomicals-electrumx) server. Inspired by https://github.com/lukechilds/docker-electrumx.


## Requirements

1. Bitcoin Full Node
2. At least **100G** left in your storage.

## Usage

### 1. Update your Bitcoin Full Node RPC settings, _SKIP_ if you already set.

Add this to your bitcoin.conf, and restart your bitcoin full node.

_assuming your lan ip is `192.168.50.2`_

```
txindex=1
rpcauth=electrumx:c7ed296134ebe0035d9ff786dfa102b5$9d40e8e36bcdba1e3ca0a79178c3864c3deaa9e6fd484ff683e7770690a97097

rpcbind=0.0.0.0
rpcallowip=127.0.0.1
rpcallowip=172.0.0.0/8
rpcallowip=192.168.50.2
```

- `txindex=1` is required for ElectrumX to work. You need wait for full node to reindex.
- You can also download script from https://github.com/bitcoin/bitcoin/blob/master/share/rpcauth/rpcauth.py. And generate one by yourself.

### 2. Clone this repo and Run the server:

```bash
git clone https://github.com/lucky2077/atomicals-electrumx-docker.git
```

```bash
cd atomicals-electrumx-docker
```

Create an `.env` file with content below:

```ini
DAEMON_URL=electrumx:electrumx@192.168.50.2:8332
```

Then run the ElectrumX server:

```bash
docker-compose pull && docker-compose up -d
```
- use `docker-compose logs -f` to check the logs.
- use `docker-compose down` to stop the server.

**NOTE**

- You should stop here until the server is fully synced.
- The `data` directory will be more than **90G** after sync.
- Testing RPC
```
url --user name:pwd --data-binary '{"jsonrpc": "1.0", "id":"curltest", "method": "getblockchaininfo", "params": [] }' -H 'content-type: text/plain;' http://localhost:8332/
```
