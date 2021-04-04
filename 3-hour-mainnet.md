# Three hours to mainnet

This is a game for three or more players. 

## Server

For servers I've really come to love:

* Hetzner.de
* Contabo (word of mouth tells me it's good, I am a hetzner man)

Go and get one.  Single core, 2 gigs of RAM is fine.  Here you go, an exact link:

https://www.hetzner.com/cloud

You need a Hetzner cx11 cloud instance. Choose nvme, not ceph. 

## Configuration of server / dev environment 

You are going to need node.js and Go, and you'll want the latest versions of both. 

So you're going to SSH into that server like:

```bash
ssh root@ipaddress
```


### Latest Go


NB: Paste lines one at a time or it could skip a step. 
```bash

sudo add-apt-repository ppa:longsleep/golang-backports

sudo apt-get update

sudo apt-get install -y git golang-go make wget

export GOROOT=/usr/lib/go
export GOPATH=${HOME}/go
export GOBIN=$GOPATH/bin
export PATH=${PATH}:${GOROOT}/bin:${GOBIN}
```


### Latest Node.js

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

Now install node using nvm, which is for installing node :)

```bash
nvm i node
```

## Latest Protobufs
```bash
export PB_REL="https://github.com/protocolbuffers/protobuf/releases"
curl -LO $PB_REL/download/v3.15.5/protoc-3.15.5-linux-x86_64.zip
unzip protoc-3.15.5-linux-x86_64.zip -d $HOME/.local
export PATH="$PATH:$HOME/.local/bin"
```

Next, check your versions:

```bash
node --version
```
v15.12.0

```bash
go version
```
go version go1.16.2 linux/amd64

```bash
protoc --version
```
libprotoc 3.6.1

## Install gh to make github smooth

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key C99B11DEB97541F0
sudo apt-add-repository https://cli.github.com/packages
sudo apt update
sudo apt install gh
```

## Install Starport
```bash
curl https://get.starport.network/starport! | bash
```


## Build your very basic blockchain

If it takes more than 20 minutes, let me (https://twitter.com/@gadikian) know, it means that something is wrong.

ensure that you have node.js, go and the github cli installed, detailed directions are provided for that above. 

**NOTE FOR TEAMS**
Only the "lead" should do the step where the chain is built and pushed to github.

```bash
# Generate a blockchain with no frills
starport app github.com/yourusernamehere/instachain
# Add super basic blogging capabilities
cd instachain
starport type post title body
git commit . -m "Added blog feature"
git add .
# Push to Github
gh auth login # only do this if you haven't logged in already
gh repo create "instachain"
git push
```

**THE PROCESS ABOVE**

1) scaffolds a fast, secure, sovereign blockchain with a simple blogging feature
2) scaffolds a modern UI for that chain
3) generates binaries on github actions
4) generates a raspberry Pi device image that includes the UI and binaries as a Github Action
5) is an illustration of the capabilities of Starport
