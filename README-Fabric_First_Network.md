# Fabric First Network

### Add fabric tooling into PATH: 

```
export PATH=/home/formssi/fabric-samples/bin:$PATH
```

### Start the fabric network with 2 organizations with 2 peers each:

```
cd /home/formssi/fabric-samples/first-network
```

Please stop any existing fabric network before you start a new network by using the following command:
```
./byfn.sh down
```
Spin up the network
```
./byfn.sh up
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli01.jpg)


### To get into fabric-tool container and set up environment variables:

```
docker exec -it cli bash 
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli02.jpg)

```
source ./scripts/setGlobalVariables.sh
```

### Switch the fabric-tool as Org1Peer0 and create channel:

```
source ./scripts/changeToOrg1Peer0.sh
```

You may use the following command to trace you acting as which peer
```
env
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli11.jpg)


```
peer channel create -o orderer.example.com:7050 -c $CHANNEL_NAME -f ./channel-artifacts/channel.tx --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli03.jpg)


### Join Org1Peer0 into the Channel:

```
peer channel join -b $CHANNEL_NAME.block
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli04.jpg)

```
peer channel list
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli12.jpg)

### Join Org1Peer1 into the Channel:

```
source ./scripts/changeToOrg1Peer1.sh
```

```
peer channel join -b $CHANNEL_NAME.block
```

### Join Org2Peer0 into the Channel:

```
source ./scripts/changeToOrg2Peer0.sh
```

```
peer channel join -b $CHANNEL_NAME.block
```

### Join Org2Peer1 into the Channel:

```
source ./scripts/changeToOrg2Peer1.sh
```

```
peer channel join -b $CHANNEL_NAME.block
```

### Install Chaincode at Org1Peer0:

```
source ./scripts/changeToOrg1Peer0.sh
```

```
peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli05.jpg)

```
peer chaincode list --installed
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli13.jpg)

### Install Chaincode at Org1Peer1:

```
source ./scripts/changeToOrg1Peer1.sh
```

```
peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
```

### Install Chaincode at Org2Peer0:

```
source ./scripts/changeToOrg2Peer0.sh
```

```
peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
```

### Install Chaincode at Org2Peer1:

```
source ./scripts/changeToOrg2Peer1.sh
```

```
peer chaincode install -n mycc -v 1.0 -l golang -p github.com/chaincode/chaincode_example02/go/
```

"github.com/chaincode/chaincode_example02/go/" is a relative path

### Instantiate Chaincode at Org1Peer0:

```
source ./scripts/changeToOrg1Peer0.sh
```

```
peer chaincode instantiate -o orderer.example.com:7050 --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -l golang -v 1.0 -c '{"Args":["init","a","100","b","200"]}' -P "AND ('Org1MSP.peer','Org2MSP.peer')"
```
-o orderer.example.com:7050: orderer address<br />
--tls: enable tls<br />
-n mycc: chaincode name<br />
-l golang: language using<br />
-v 1.0: chaincode version<br />
-P "AND ('Org1MSP.peer','Org2MSP.peer')": endorsement policy<br />

note: 
* instantiate will create a docker container which may consume more time
* you just need to instantiate chaincode once, other chaincode container will be generated on the fly when needed
(Tips: invoke query during deployment)

### Query Chaincode at Org1Peer0:

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli06.jpg)

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli07.jpg)

### Query Chaincode at Org1Peer1 (this may take:

```
source ./scripts/changeToOrg1Peer1.sh
```

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'
```

### Query Chaincode at Org2Peer0:

```
source ./scripts/changeToOrg2Peer0.sh
```

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'
```

### Query Chaincode at Org2Peer1:

```
source ./scripts/changeToOrg2Peer1.sh
```

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'
```

### Invoke Chaincode at Org1Peer0:

```
source ./scripts/changeToOrg1Peer0.sh
```

```
peer chaincode invoke -o orderer.example.com:7050 --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc --peerAddresses peer0.org1.example.com:7051 --tlsRootCertFiles $PEER0_ORG1_CA --peerAddresses peer0.org2.example.com:9051 --tlsRootCertFiles $PEER0_ORG2_CA -c '{"Args":["invoke","a","b","10"]}'
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli08.jpg)

--peerAddresses: indicate which peer will be used for endorsement

### Check the result by Query Chaincode at Org2Peer1:

```
source ./scripts/changeToOrg2Peer1.sh
```

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli09.jpg)

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli10.jpg)

### Try to Invoke Chaincode with only one peerAddress at Org1Peer0:

```
source ./scripts/changeToOrg1Peer0.sh
```

```
peer chaincode invoke -o orderer.example.com:7050 --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc --peerAddresses peer0.org1.example.com:7051 --tlsRootCertFiles $PEER0_ORG1_CA -c '{"Args":["invoke","a","b","10"]}'
```

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","a"]}'
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli09.jpg)

```
peer chaincode query -C $CHANNEL_NAME -n mycc -c '{"Args":["query","b"]}'
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli10.jpg)

Exit the cli tool:
```
exit
```

```
docker ps
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli16.jpg)

```
docker logs e7c1a026b8e0
```
![](https://github.com/janeleung0802/blockchain/blob/master/cli14.jpg)


recall: When we instantiate the chaincode we have specified that we need both peer of Org1 and Org2 for endorsement

![](https://github.com/janeleung0802/blockchain/blob/master/cli15.jpg)








