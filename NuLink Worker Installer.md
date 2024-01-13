
# NuLink Testnet Horus 2.0

![nlk](https://github.com/Lorento34/NuLink-Testnet-Horus-2.0/assets/84406096/5942336a-d881-4c50-8504-63fc6609c957)

Before Proceeding Make sure you Finish the Staking Task First:
https://github.com/Lorento34/NuLink-Testnet-Horus-2.0/blob/main/NuLink%20for%20Stakers.md

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
ufw enable
ufw allow ssh
ufw allow https
ufw allow http
ufw allow 9151

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

- Remark: Save the output of the public address of the key and the path to the Secret key file in a text document. You will use it in step 9 and step 12.
  
```
./geth account new --keystore ./keystore
```

Example: Sample output should be as follows.
```
INFO [09-08|15:30:11.904] Maximum peer count                       ETH=50 LES=0 total=50
INFO [09-08|15:30:11.905] Smartcard socket not found, disabling    err="stat /run/pcscd/pcscd.comm: no such file or directory"
Your new account is locked with a password. Please give a password. Do not forget this password.
Password: 
Repeat password: 

Your new key was generated


Public address of the key:   0x8B1819341BEc211a45a2186C4D0030681cccE0Ee
Path of the secret key file: /root/geth-linux-amd64-1.10.23-d901d853/keystore/UTC--2022-09-13T01-14-32.465358210Z--8b1819341bec211a45a2186c4d0030681ccce0ee

- You can share your public address with anyone. Others need it to interact with you.
- You must NEVER share the secret key with anyone! The key controls access to your funds!
- You must BACKUP your key file! Without the key, it's impossible to access account funds!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
```

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
cd nulink

```
9- In step 5, you should edit the path to the secret key file given to you with the code below.

```
cp /root/geth-linux-amd64-1.10.23-d901d853/keystore/”Insert the secret key file here” /root/nulink
```

Example:
```
cp /root/geth-linux-amd64-1.10.23-d901d853/keystore/UTC--2022-09-13T01-14-32.465358210Z--8b1819341bec211a45a2186c4d0030681ccce0ee /root/nulink
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

- Remark 1: You need to claim some BNB(test) token for Worker account as gas fee.
- Remark 2: If you encounter error when starting Worker node, first please check that the port 9151 has not been occupied by other process.
  
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
15- Send tBNB in your Worker Address, and it will work now.
Working Logs Sample will Inout:
Confirming operator address 0xbd4F8C5E is already confirmed
learn_from_teacher_node stop now RELAX.

16. Apply in the Form Below.
https://docs.google.com/forms/d/e/1FAIpQLSdY2eXwQD-tKvJ_Ug-6hgdcWK_wUOZjXeJknw5XWSEO8gzJ2w/viewform

Take Note: You need to run it up to 8 Epoch and needs up to 80% UpTime.
Good luck! Paldow! 
