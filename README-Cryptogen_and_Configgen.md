# Cryptogen and Configgen

### Move to first network directory:

1. cd ~/fabric-sample/first-network

### Move to first network directory:
1. rm -rf channel-artifacts/

2. rm -rf crypto-config

### Generate crypto:
1. export CHANNEL_NAME=mychannel

2. cryptogen generate --config=./crypto-config.yaml

### Generate the Channel Archifact:
1. mkdir -p channel-artifacts

2. configtxgen -profile TwoOrgsOrdererGenesis -channelID byfn-sys-channel -outputBlock ./channel-artifacts/genesis.block

3. configtxgen -profile TwoOrgsChannel -outputCreateChannelTx ./channel-artifacts/channel.tx -channelID $CHANNEL_NAME