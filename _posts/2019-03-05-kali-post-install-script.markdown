---
layout: post
title: Kali post install script
subtitle: "shell script"
date: 2019-03-05
background: '/PATH_TO_IMAGE'
---


### Adding extra stuff to Kali

Kali linux comes with a wide array of tools by default but I still find myself adding additional tools.  I know you can use things like PTF (pen test framework),
but I typically don't need everything it installs.

I am hopeing to maintain a shell script that does that for my after I spin up a new instance of kali. I will keep it updated and add a git repo for it soon.

```bash
!#/bin/bash
## Go lang install and add /home/go/bin to path
apt-get install -y golang
echo 'export GOROOT=/usr/lib/go' >> ~/.bashrc
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
echo 'export PATH=$PATH:$GOROOT/bin:$GOPATH/bin' >> ~/.bashrc

## Aquatone
go get github.com/michenriksen/aquatone

## Bloodhound
apt-get install -y bloodhound
	##Neo4j - change password
	##neo4j console

## Crackmapexec - Dev version
apt-get install -y libssl-dev libffi-dev python-dev build-essential
pip install --user pipenv
cd /opt/
git clone --recursive https://github.com/byt3bl33d3r/CrackMapExec
cd CrackMapExec && pipenv install
pipenv shell
python setup.py install
exit

## EAP Hammer
cd /opt
git clone https://github.com/s0lst1c3/eaphammer.git
cd eaphammer
./kali-setup

## Gitrob
go get github.com/michenriksen/gitrob
echo 'export GITROB_ACCESS_TOKEN=' >> ~/bashrc

## Gobuster
go get github.com/OJ/gobuster

## impacket
cd /opt
git clone https://github.com/SecureAuthCorp/impacket.git
cd impacket
pip install .

## King-phisher
cd /tmp
wget -q https://github.com/securestate/king-phisher/raw/master/tools/install.sh && \
bash ./install.sh

## Ruler
go get github.com/sensepost/ruler

## Wordlist
cd /opt
git clone https://github.com/danielmiessler/SecLists.git
wget https://gist.githubusercontent.com/jhaddix/b80ea67d85c13206125806f0828f4d10/raw/c81a34fe84731430741e0463eb6076129c20c4c0/content_discovery_all.txt
wget https://gist.githubusercontent.com/jhaddix/86a06c5dc309d08580a018c66354a056/raw/96f4e51d96b2203f19f6381c8c545b278eaa0837/all.txt

## Wifite2
cd /opt
git clone https://github.com/derv82/wifite2.git
git clone https://github.com/ZerBea/hcxdumptool.git
cd hcxdumptool
make
make install
cd /opt
git clone https://github.com/ZerBea/hcxtools.git
cd hcxtools
make
make install

## wpscan
apt-get install -y wpscan
```