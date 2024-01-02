
# NuLink Testnet Horus 2.0

![nlk](https://github.com/Lorento34/NuLink-Testnet-Horus-2.0/assets/84406096/5942336a-d881-4c50-8504-63fc6609c957)


<h1>Minimum System Requirements<h6>

 - Debian/Ubuntu 20.04 (Recommended)
 - 4 GB Ram
 - 30GB Available Storage
 - Minimum 2 CPU processors
 - x86 Architecture
 - Static IP address
 - Exposed TCP port 9151, make sure it's not occupied
 - Nodes can be run on cloud infrastructure

<h1>NuLink Worker Installer steps for Ubuntu 20.4<h6>

1- First of all, if you are going to install outside of vps providers where ports such as Contabo, Hetzner, Linode, Digital Oceon are open. You must open port 9151 with the codes below or from the virtual server provider's own site.

```
apt install ufw -y
```
```
ufw allow ssh
ufw allow https
ufw allow http
ufw allow 9151
ufw enable
```

2- Download Geth on your server.
 
```
wget https://gethstore.blob.core.windows.net/builds/geth-linux-amd64-1.10.23-d901d853.tar.gz
```

3- Unzip the downloaded installation package.

```
tar -xvzf geth-linux-amd64-1.10.23-d901d853.tar.gz
```

4- Enter the unzipped directory

```
cd geth-linux-amd64-1.10.23-d901d853/
```

5- Create Ethereum account and keystore. You will be asked to enter and confirm the password. Please remember this password for future use. 

- Remark2: Save the output of the public address of the key and the path to the Secret key file in a text document. You will use it in step 9 and step 12.
  
```
./geth account new --keystore ./keystore
```

Example: Sample output should be as follows.

![keys](https://github.com/Lorento34/NuLink-Testnet-Horus-2.0/assets/84406096/b41caa54-2d62-47fb-a8ee-f861fd6ca894)


6- Docker installation

```
sudo apt-get update
```
```
sudo apt-get install ca-certificates curl gnupg
```
```
sudo install -m 0755 -d /etc/apt/keyrings
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```
sudo apt-get update
```
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

7- Pull the latest NuLink image.

```
docker pull nulink/nulink:latest
```

8- Create a directory in your host machine for later usage.

```
cd /root
mkdir nulink
```

9- In step 5, you should edit the path to the secret key file given to you with the code below.

```
cp /root/geth-linux-amd64-1.10.23-d901d853/keystore/* /root/nulink
```

Example:
```
cp /root/codespaces-blank/geth-linux-amd64-1.10.23-d901d853/keystore/UTC--2023-12-31T17-42-14.316243885Z--f3defb90c2f03e904bd9662a1f16dcd1ca69b00a /root/nulink
```

10- Please ensure that this directory has 777 permissions.

```
chmod -R 777 /root/nulink
```

11- Select a password with at least 8 characters to lock and unlock the private storage created by the NuLink Worker. It's important to remember this password for future access.
important information, use the password you created earlier. And edit the following codes according to your password.

```
export NULINK_KEYSTORE_PASSWORD=<YOUR NULINK STORAGE PASSWORD>
```

```
export NULINK_OPERATOR_ETH_PASSWORD=<YOUR WORKER ACCOUNT PASSWORD>
```

Example:

```
export NULINK_KEYSTORE_PASSWORD=12345678
```

```
export NULINK_OPERATOR_ETH_PASSWORD=12345678
```

12- Initialize Node Configuration. You will configure the configuration according to your own information.

```
docker run -it --rm \
-p 9151:9151 \
-v </path/to/host/machine/directory>:/code \
-v </path/to/host/machine/directory>:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer <ETH KEYSTORE URI> \
--eth-provider <NULINK PROVIDER URI>  \
--network <NULINK NETWORK NAME> \
--payment-provider <PAYMENT PROVIDER URI> \
--payment-network <PAYMENT NETWORK NAME> \
--operator-address <WORKER ADDRESS> \
--max-gas-price <GWEI>
```

Example:

```
docker run -it --rm \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
nulink/nulink nulink ursula init \
--signer keystore:///code/UTC--2023-12-31T17-42-14.316243885Z--f3defb90c2f03e904bd9662a1f16dcd1ca69b00a \
--eth-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--network horus \
--payment-provider https://data-seed-prebsc-2-s2.binance.org:8545 \
--payment-network bsc_testnet \
--operator-address 0xf3defb90c2f03e904bd9662a1f16dcd1ca69b00a \
--max-gas-price 10000000000
```


13- Launch the Node. The following command will start the node. Make sure you use the same host directory as the configuration.

- Remark1: You need to claim some BNB(test) token for Worker account as gas fee.
- Remark2: If you encounter error when starting Worker node, first please check that the port 9151 has not been occupied by other process. If still not working, please check there is only one configuration json

 file in the </path/to/host/machine/directory>

```
docker run --restart on-failure -d \
--name ursula \
-p 9151:9151 \
-v /root/nulink:/code \
-v /root/nulink:/home/circleci/.local/share/nulink \
-e NULINK_KEYSTORE_PASSWORD \
-e NULINK_OPERATOR_ETH_PASSWORD \
nulink/nulink nulink ursula run --no-block-until-ready
```

14- Check Node Status for Worker Account.
```
docker logs -f ursula
```












