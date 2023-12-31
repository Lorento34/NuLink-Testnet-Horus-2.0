# NuLink Testnet Horus 2.0
About installing a node


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
sudo ufw enable
sudo ufw allow 9151
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

5- Use. / get account new -- keystore. / keystore to generate Ethereum account and keystore

```
./geth account new --keystore ./keystore
```

6- After using ./geth account new --keystore ./keystore you will be asked to enter and confirm the password. Please remember this password for future use. Sample output should be as follows.

![keys](https://github.com/Lorento34/NuLink-Testnet-Horus-2.0/assets/84406096/b41caa54-2d62-47fb-a8ee-f861fd6ca894)


7- Docker installation

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

8- Pull the latest NuLink image.

```
docker pull nulink/nulink:latest
```

9- Create a directory in your host machine for later usage.

```
cd /root
mkdir nulink
```

10- In step 6, you should edit the path to the secret key file given to you with the code below.

```
cp /root/geth-linux-amd64-1.10.23-d901d853/keystore/* /root/nulink
```

Example:
```
cp /root/codespaces-blank/geth-linux-amd64-1.10.23-d901d853/keystore/UTC--2023-12-31T17-42-14.316243885Z--f3defb90c2f03e904bd9662a1f16dcd1ca69b00a /root/nulink
```

11- Please ensure that this directory has 777 permissions.

```
chmod -R 777 /root/nulink
```

12- Install Python.

```
apt install python3-pip
```

13- Install virtual environment.
```
pip install virtualenv
```

14- Create a Virtual Environment.
```
virtualenv /root/nulink-venv
```

15- Activate the newly created virtual environment.
```
source /root/nulink-venv/bin/activate
```
16- Install the Nulink python package.

```
wget https://download.nulink.org/release/core/nulink-0.5.0-py3-none-any.whl
```

17- Install the python package.
```
pip install nulink-0.5.0-py3-none-any.whl
```

18- Before Verifying Setup, verify that your Nulink setup and entry points are functional.
```
source /root/nulink-venv/bin/activate
```
```
python -c "import nulink"
```
After entering the nulink --help command, you will get the following output

```
nulink --help
```
Exmple:

![help](https://github.com/Lorento34/NuLink-Testnet-Horus-2.0/assets/84406096/bb7cc207-5d21-4090-a7ca-b9baebf63da4)


19- Select a password with at least 8 characters to lock and unlock the private storage created by the NuLink Worker. It's important to remember this password for future access.
important information, use the password you created earlier. And edit the following codes according to your password

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























