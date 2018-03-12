# GoByte
Building Your GoByte Master Node

## Here is a Full Video Walk-through
Coming Soon

## Preperation

1. Get a VPS from a provider like DigitalOcean, AWS, Vultr, Linode, etc. 
   - Recommended VPS size at least 1gb RAM 
   - **It must be Ubuntu 16.04 (Xenial)**
2. Make sure you have a transaction of exactly 1000 GBX in your desktop cold wallet.

## Cold Wallet Setup Part 1

1. Open your wallet on your desktop.

   Click Settings -> Options -> Wallet
   
   Check "Enable coin control features"
   
   Check "Show Masternodes Tab"
   
   Press Ok (you will need to restart the wallet)
   
   ![Alt text](https://github.com/tokayseo/Civitas/blob/master/GBX.PNG "Wallet Settings")

   
   
   
2. Go to the "Tools" -> "Debug console"
3. Run the following command: `masternode genkey`
4. You should see a long key that looks like:
```
3HaYBVUCYjEMeeH1Y4sBGLALQZE1Yc1K64xiqgX37tGBDQL8Xg
```
5. This is your `private key`, keep it safe, do not share with anyone.




## VPS Setup

1. Log into your VPS

2. Copy/paste these commands into the VPS and hit enter: (One Box At A Time)
```
apt-get -y update
```
```
apt-get -y upgrade
```
```
apt-get -y install build-essential libtool autotools-dev autoconf pkg-config libssl-dev
```
```
apt-get -y install software-properties-common
```
```
add-apt-repository -y ppa:bitcoin/bitcoin
```
```
apt-get -y update
```
```
apt-get -y install libdb4.8-dev libdb4.8++-dev
```
```
apt-get -y install libboost-all-dev
```
```
apt-get -y install libminiupnpc-dev
```
```
apt-get -y install libevent-dev
```
```
apt-get -y install git
```
```
git clone https://github.com/gobytecoin/gobyte
```
```
cd gobyte
```
```
apt-get -y install automake
```
```
./autogen.sh
```
```
./configure
```
```
For Digital Ocean:
cd /var
touch swap.img
chmod 600 swap.img
dd if=/dev/zero of=/var/swap.img bs=1024k count=2000
mkswap /var/swap.img
swapon /var/swap.img
echo "/var/swap.img none swap sw 0 0" >> /etc/fstab

For Other VPS:
fallocate -l 2G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
sudo nano /etc/fstab
```

At the bottom of the file, you need to add a line that will tell the
operating system to automatically use the file you created:

```
/swapfile none swap sw 0 0
```
```
make
```
```
cd src
strip gobyte-tx
strip gobyte-cli
strip gobyted
mv gobyte{d,-cli} /usr/local/bin
cd ~/
rm -r gobyte
```
```
cd
```
```
mkdir -p .gobyte
```
```
nano .gobyte/gobyte.conf
```
Replace:
externalip=VPS_IP_ADDRESS
masternodeprivkey=WALLET_GENKEY
With your info!
```
rpcuser=randomname
rpcpassword=awesomepassword
rpcallowip=127.0.0.1
listen=1
server=1
daemon=1
logtimestamps=1
maxconnections=256
masternode=1
externalip=VPS_IP_ADDRESS:
masternodeprivkey=WALLET_GENKEY
```
CTRL X to save it. Y for yes, then ENTER.
```
gobyted &
```

3.Use `watch gobyte-cli getinfo` to check and wait til it's synced 



## Cold Wallet Setup Part 2 

1. On your local machine open your `masternode.conf` file.
   Depending on your operating system you will find it in:
   * Windows: `%APPDATA%\gobyte\`
   * Mac OS: `~/Library/Application Support/gobyte/`
   * Unix/Linux: `~/.gobyte/`
   
   Leave the file open
2. Go to "Tools" -> "Debug console"
3. Run the following command: `masternode outputs`
4. You should see output like the following if you have a transaction with exactly 1000 GBX:
```
{
    "12345678xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx": "0"
}
```
5. The value on the left is your `txid` and the right is the `vout`
6. Add a line to the bottom of the already opened `masternode.conf` file using the IP of your
VPS (with port 24126), `private key`, `txid` and `vout`:
```
mn1 1.2.3.4:24126 3xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 12345678xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 0 
```
7. Save the file, exit your wallet and reopen your wallet.
8. Go to the "Masternodes" tab
9. Click "Start All"

Cheers !!
