# Change the chaincode and upgrade

## Practice

Add a “delete” function (delete a specific account of a party) at chancode “/home/formssi/fabric-samples/chaincode/chaincode_example02/go/ chaincode_example02.go
### Upgrade the Chaincode:

#### Install Chaincode 2.0 at Org1Peer0:

1. source ./scripts/changeToOrg1Peer0.sh
2. peer chaincode install -n mycc -v 2.0 -l golang -p github.com/chaincode/chaincode_example02/go/

#### Install Chaincode 2.0 at Org1Peer1:

1. source ./scripts/changeToOrg1Peer1.sh
2. peer chaincode install -n mycc -v 2.0 -l golang -p github.com/chaincode/chaincode_example02/go/

#### Install Chaincode 2.0 at Org2Peer0:
1. source ./scripts/changeToOrg2Peer0.sh
2. peer chaincode install -n mycc -v 2.0 -l golang -p github.com/chaincode/chaincode_example02/go/

#### Install Chaincode 2.0 at Org2Peer1:
1. source ./scripts/changeToOrg2Peer1.sh
2. peer chaincode install -n mycc -v 2.0 -l golang -p github.com/chaincode/chaincode_example02/go/

#### Upgrade Chaincode 2.0 at Org1Peer0:
1. source ./scripts/changeToOrg1Peer0.sh
2. peer chaincode upgrade -o orderer.example.com:7050 --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -v 2.0 -c '{"Args":["init","a","100","b","200"]}' -P "AND ('Org1MSP.peer','Org2MSP.peer')"

