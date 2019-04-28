# Fabric First Network

### Add fabric tooling into PATH: 

export PATH=/home/formssi/fabric-samples/bin:$PATH

### Start the fabric network with 2 organizations with 2 peers each:

1. cd /home/formssi/fabric-samples/first-network

2. ./byfn.sh up
![](https://github.com/janeleung0802/blockchain/blob/master/cli01.JPG)


### To get into fabric-tool container and set up environment variables:

1. docker exec -it cli bash 
![](https://github.com/janeleung0802/blockchain/blob/master/cli02.JPG)

2. source ./scripts/setGlobalVariables.sh


### Switch the fabric-tool as Org1Peer0 and create channel:

1. source ./scripts/changeToOrg1Peer0.sh

2. peer channel create -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/channel.tx --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA
![](https://github.com/janeleung0802/blockchain/blob/master/cli03.JPG)


### Join Org1Peer0 into the Channel:

peer channel join -b $CHANNEL_NAME.block
![](https://github.com/janeleung0802/blockchain/blob/master/cli04.JPG)

### Join Org1Peer1 into the Channel:

1. source ./scripts/changeToOrg1Peer1.sh

2. peer channel join -b $CHANNEL_NAME.block

### Join Org2Peer0 into the Channel:

1. source ./scripts/changeToOrg2Peer0.sh

2. peer channel join -b $CHANNEL_NAME.block

### Join Org2Peer1 into the Channel:

1. source ./scripts/changeToOrg2Peer1.sh

2. peer channel join -b $CHANNEL_NAME.block


### Install Chaincode at Org1Peer0:

1. source ./scripts/changeToOrg1Peer0.sh

2. peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
![](https://github.com/janeleung0802/blockchain/blob/master/cli05.JPG)


### Install Chaincode at Org1Peer1:

1. source ./scripts/changeToOrg1Peer1.sh

2. peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/

### Install Chaincode at Org2Peer0:

1. source ./scripts/changeToOrg2Peer0.sh

2. peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/


### Install Chaincode at Org2Peer1:

1. source ./scripts/changeToOrg2Peer1.sh

2. peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/


### Instantiate Chaincode at Org1Peer0:

1. source ./scripts/changeToOrg1Peer0.sh

2. peer chaincode instantiate -o orderer.example.com:7050 --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -l golang -v 1.0 -c '{"Args":["init","a","100","b","200"]}' -P "AND ('Org1MSP.peer','Org2MSP.peer')"


### Query Chaincode at Org1Peer0:

1. peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
![](https://github.com/janeleung0802/blockchain/blob/master/cli06.JPG)

2. peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'
![](https://github.com/janeleung0802/blockchain/blob/master/cli07.JPG)

### Query Chaincode at Org1Peer1 (this may take:

1. source ./scripts/changeToOrg1Peer1.sh

2. peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'

3. peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'

### Query Chaincode at Org2Peer0:

1. source ./scripts/changeToOrg2Peer0.sh

2. peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'

3. peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'

### Query Chaincode at Org2Peer1:

1. source ./scripts/changeToOrg2Peer1.sh

2. peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'

3. peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'

### Invoke Chaincode at Org1Peer0:
1. source ./scripts/changeToOrg1Peer0.sh

2. peer chaincode invoke -o orderer.example.com:7050 --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc --peerAddresses peer0.org1.example.com:7051 --tlsRootCertFiles $PEER0_ORG1_CA --peerAddresses peer0.org2.example.com:9051 --tlsRootCertFiles $PEER0_ORG2_CA -c '{"Args":["invoke","a","b","10"]}'
![](https://github.com/janeleung0802/blockchain/blob/master/cli08.JPG)

### Check the result by Query Chaincode at Org2Peer1:

1. source ./scripts/changeToOrg2Peer1.sh

2. peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
![](https://github.com/janeleung0802/blockchain/blob/master/cli09.JPG)

3. peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'
![](https://github.com/janeleung0802/blockchain/blob/master/cli10.JPG)
