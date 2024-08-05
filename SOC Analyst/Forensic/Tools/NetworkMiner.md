**Install on Kali**
Prerequisites
```shell
sudo apt install mono-devel
```

Download and Configure
```shell
wget https://www.netresec.com/?download=NetworkMiner -O /tmp/NetworkMiner.zip
sudo unzip /tmp/NetworkMiner.zip -d /opt/
cd /opt/NetworkMiner*
sudo chmod +x NetworkMiner.exe
sudo chmod -R go+w AssembledFiles/
sudo chmod -R go+w Captures/
```

Launch NetworkMiner
```shell
mono /opt/NetworkMiner*/NetworkMiner.exe
```

