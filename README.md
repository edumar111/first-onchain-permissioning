# first-onchain-permissioning
    
    https://besu.hyperledger.org/stable/private-networks/tutorials/permissioning/onchain

### Prerequisites
    - java 17
    - besu/v23.4.1
    - Node.js v10.16.0
    - Yarn v1.15 


# iinstall java 17 
    apt update
    sudo apt install openjdk-17-jdk -y
## install besu on ubuntu 22

    https://hyperledger.jfrog.io/artifactory/besu-binaries/besu/{{ besu_release_version}}/besu-{{ besu_release_version}}.tar.gz


    $ wget https://hyperledger.jfrog.io/artifactory/besu-binaries/besu/23.4.1/besu-23.4.1.tar.gz

    $ tar –xvzf besu-23.4.1.tar.gz 
    $ mv ./besu-23.4.1 /usr/local/besu
   
## config besu
    nano .bashrc
    export PATH=/usr/local/besu/bin:$PATH
    source .bashrc


## generacion de extrada 
    $ besu operator generate-blockchain-config --config-file=ibftConfigFile.json --to=networkFiles --private-key-file-name=key

Ejemplos:
```
con tres nodos validadores

"extraData" : "0xf87ea00000000000000000000000000000000000000000000000000000000000000000f85494
    e35801dc797250dac5ec8ebfcaf6dde32b0b9f35 94
    46dd61b6819e3154a4dac13c798a8165fb4db1aa 94
    a937decd2b1548ef76c9a2dd96ae0a87d964cd21 94
    0ef93fa7001807072be25852e3feccf6060f8298
    8acc0557cc2f3e5c953693bc0e517ca40b050b29
    808400000000c0"
 
de mainnet   

"extraData" : "0xf83ea00000000000000000000000000000000000000000000000000000000000000000d594
    8acc0557cc2f3e5c953693bc0e517ca40b050b29
    808400000000c0"

ejemplo con un validador

"extraData" : "0xf83ea00000000000000000000000000000000000000000000000000000000000000000d594
     5a0430436c87af705448973f020c8d60d66e7007
     808400000000c0"
```
## add the Ingress contracts to the genesis file

    https://besu.hyperledger.org/23.4.0/public-networks/reference/genesis-items#milestone-blocks

## LEVANTAR LA RED PARA GENERAR LOS  PRIMERO BLOQUEAS ANTES DE DESPLEGAR LOS CONTRATOS DE PERMISIONADO
### RUN NODE 1 in folder Node-1

    besu --data-path=data --genesis-file=../genesis.json --permissions-accounts-contract-enabled=false --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled=false  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*"

enode://d1e9fb17664ad4ac5ef275c97b2e984b25d8de9a7618a06031dcd68d46e596b8117f66fa10f75bf8d5d3229e366d97514c86981ee14c1787b5a1d373573e984e@127.0.0.1:30303

Node address 0x0ef93fa7001807072be25852e3feccf6060f8298

## Run Node 2

    besu --data-path=data --genesis-file=../genesis.json --bootnodes=enode://d1e9fb17664ad4ac5ef275c97b2e984b25d8de9a7618a06031dcd68d46e596b8117f66fa10f75bf8d5d3229e366d97514c86981ee14c1787b5a1d373573e984e@127.0.0.1:30303 --permissions-accounts-contract-enabled=false --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled=false  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --p2p-port=30304 --rpc-http-port=8546


 Enode URL enode://08da11e012801d9b438bba854ce053068e416b5765dddc183be1bf9f06cd41f5af0223b3bec03fe07f0f17da7fa02755bfbdf1a089668682b121add12fbe3a18@127.0.0.1:30304

 Node address 0x46dd61b6819e3154a4dac13c798a8165fb4db1aa

## Run Node 3

    besu --data-path=data --genesis-file=../genesis.json --bootnodes=enode://d1e9fb17664ad4ac5ef275c97b2e984b25d8de9a7618a06031dcd68d46e596b8117f66fa10f75bf8d5d3229e366d97514c86981ee14c1787b5a1d373573e984e@127.0.0.1:30303  --permissions-accounts-contract-enabled=false --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled=false  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --p2p-port=30305 --rpc-http-port=8547


 enode://e1498502e0dbd770894ce577d05bd7ea40a2963520fca337f01ae4cbfbf2a9e255051cd9babb4691f3fc6caba0544f2ad9c422fa48adbedf172bdca12b1d3202@127.0.0.1:30305
 Node address 0xa937decd2b1548ef76c9a2dd96ae0a87d964cd21

## Run Node 4
    besu --data-path=data --genesis-file=../genesis.json --bootnodes=enode://d1e9fb17664ad4ac5ef275c97b2e984b25d8de9a7618a06031dcd68d46e596b8117f66fa10f75bf8d5d3229e366d97514c86981ee14c1787b5a1d373573e984e@127.0.0.1:30303 --permissions-accounts-contract-enabled=false --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled=false  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --p2p-port=30306 --rpc-http-port=8548



Enode URL enode://ef9481a9a4694d91e906ff2793d1113e9c72b845350400e42e0a08c98c476fe03f91cf576fb449bc37afb9372b72451aefad14f4823ae6ab5ad9b27478a6bc0f@127.0.0.1:30306
2024-03-14 17:24:45.379+00:00 | main | INFO  | DefaultP2PNetwork | Node address 0xe35801dc797250dac5ec8ebfcaf6dde32b0b9f35

## limpiar cache de yarn
    rm -rf node_modules **/node_modules
    rm -rf yarn.lock **/yarn.lock
    yarn cache clean
    yarn install


## hex to string
    https://codebeautify.org/hex-string-converter

    0x72756c6573000000000000000000000000000000000000000000000000000000 --> rules
    0x61646d696e697374726174696f6e000000000000000000000000000000000000 --> administration

## desplegar los contratos
copie los node_modules

    yarn truffle migrate --reset --network besu    

## compilar y correr la dapps
    yarn run build
    yarn run start

    http://localhost:3000

##   permisionamos los enodes en la dapss===================================================================

## despues de deplegar los contratos activamos **permissions-nodes-contract-enabled**

### node 1

     besu --data-path=data --genesis-file=../genesis.json --revert-reason-enabled --permissions-accounts-contract-enabled=false --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=DEBUG,TRACE,ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*"

### node 2
    besu --data-path=data --genesis-file=../genesis.json --bootnodes=enode://4cfa4e1a8a52f5b8334939b0a31ff375fbd95d08f96899888f016ccc92466f50148c06a31c1069bc7c12010c0d356b6c36afb6cd68dcdb7ae5b534e80c4897b5@127.0.0.1:30303 --permissions-accounts-contract-enabled=false --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --p2p-port=30304 --rpc-http-port=8546

### node 3

    besu --data-path=data --genesis-file=../genesis.json --bootnodes=enode://4cfa4e1a8a52f5b8334939b0a31ff375fbd95d08f96899888f016ccc92466f50148c06a31c1069bc7c12010c0d356b6c36afb6cd68dcdb7ae5b534e80c4897b5@127.0.0.1:30303 --permissions-accounts-contract-enabled=false --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --p2p-port=30305 --rpc-http-port=8547

### node 4

    besu --data-path=data --genesis-file=../genesis.json --bootnodes=enode://4cfa4e1a8a52f5b8334939b0a31ff375fbd95d08f96899888f016ccc92466f50148c06a31c1069bc7c12010c0d356b6c36afb6cd68dcdb7ae5b534e80c4897b5@127.0.0.1:30303 --permissions-accounts-contract-enabled=false --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --p2p-port=30306 --rpc-http-port=8548



### Activando permissions-accounts-contract=====================================================

### node 1
    besu --data-path=data --genesis-file=../genesis.json --revert-reason-enabled --permissions-accounts-contract-enabled --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=DEBUG,TRACE,ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*"

### node 2
    besu --data-path=data --genesis-file=../genesis.json --bootnodes=enode://4cfa4e1a8a52f5b8334939b0a31ff375fbd95d08f96899888f016ccc92466f50148c06a31c1069bc7c12010c0d356b6c36afb6cd68dcdb7ae5b534e80c4897b5@127.0.0.1:30303 --permissions-accounts-contract-enabled --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --p2p-port=30304 --rpc-http-port=8546

### node 3

    besu --data-path=data --genesis-file=../genesis.json --bootnodes=enode://4cfa4e1a8a52f5b8334939b0a31ff375fbd95d08f96899888f016ccc92466f50148c06a31c1069bc7c12010c0d356b6c36afb6cd68dcdb7ae5b534e80c4897b5@127.0.0.1:30303 --permissions-accounts-contract-enabled --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --p2p-port=30305 --rpc-http-port=8547

### node 4

    besu --data-path=data --genesis-file=../genesis.json --bootnodes=enode://4cfa4e1a8a52f5b8334939b0a31ff375fbd95d08f96899888f016ccc92466f50148c06a31c1069bc7c12010c0d356b6c36afb6cd68dcdb7ae5b534e80c4897b5@127.0.0.1:30303 --permissions-accounts-contract-enabled --permissions-accounts-contract-address "0x0000000000000000000000000000000000008888" --permissions-nodes-contract-enabled  --permissions-nodes-contract-address "0x0000000000000000000000000000000000009999" --permissions-nodes-contract-version=2 --rpc-http-enabled --rpc-http-cors-origins="*" --rpc-http-api=ADMIN,ETH,NET,PERM,IBFT --host-allowlist="*" --p2p-port=30306 --rpc-http-port=8548