# first-onchain-permissioning

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


