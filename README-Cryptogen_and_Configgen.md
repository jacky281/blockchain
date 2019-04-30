# Cryptogen and Configgen

### Move to first network directory:

```
cd ~/fabric-sample/first-network
```

### Move to first network directory:

```
rm -rf channel-artifacts/
```

```
rm -rf crypto-config
```

### Generate crypto:

```
export CHANNEL_NAME=mychannel
```

```
cryptogen generate --config=./crypto-config.yaml
```

### Generate the Channel Archifact:

```
mkdir -p channel-artifacts
```

```
configtxgen -profile TwoOrgsOrdererGenesis -channelID byfn-sys-channel -outputBlock ./channel-artifacts/genesis.block
```

configtx.yaml : channel configuration file 
channelID : channel to generate genesis.block
genesis.block : Blockchain's first block, which is an empty block. Required for hashing purpose.

3. configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID $CHANNEL_NAME

channelID : real channel used for communication
channel.tx : a packaged channel config, can be used by "peer channel create" command to generate a channel