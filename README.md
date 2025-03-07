
![le ebin logo](nntpchan.png "ebin logo")

![MIT License](https://img.shields.io/github/license/majestrate/nntpchan.svg)
![Logo is ebin](https://img.shields.io/badge/logo-ebin-brightgreen.svg)


**NNTPChan** (previously known as overchan) is a decentralized imageboard that uses the [NNTP protocol](https://en.wikipedia.org/wiki/Network_News_Transfer_Protocol) (network-news transfer protocol) to synchronize content between many different servers. It utilizes cryptographically signed posts to perform optional/opt-in decentralized moderation.

## Getting started Ubuntu 20.04 installation guide

[This](doc) is a step-by-step guide for getting up-and-running with NNTPChan as well as documentation for developers who want to either work on NNTPChan directly or use NNTPChan in their aplications with the API.

### Step 1: Download Go 1.15 Tarball
```
wget https://dl.google.com/go/go1.15.linux-amd64.tar.gz
```

### Step 2: Extract the Tarball
```
sudo tar -C /usr/local -xvzf go1.15.linux-amd64.tar.gz
```

### Set Up Go Environment Variables
```
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

### Install the dependancies

    sudo apt-get update
    sudo apt-get --no-install-recommends install imagemagick ffmpeg sox build-essential git ca-certificates postgresql postgresql-client golang
    

### Get the NNTPChan source
    git clone https://github.com/konamicode9/nntpchan
    cd nntpchan

### Now compile!

Run `make`:

    make

## now its time to create the database in postgres
```
CREATE DATABASE root;
CREATE USER root WITH PASSWORD 'root';
GRANT ALL PRIVILEGES ON DATABASE root TO root;
```
note this onky allows db root and username root not sure why but i will investigate oh and the default port is 5432
your gonna need it in the installation

Running NNTPChan
================

Once you have [built NNTPChan](building.md) and done [the initial setup you](setting-up.md) you can start NNTPChan.

Before running make sure you run the setup command, you only need to do this one time:

    ./srndv2 setup

You can now start the NNTPChan node (srndv2) by running:

    ./srndv2 run

Now you can check out the web-interface by navigating to 127.0.0.1:18000 (default address - unless you have changed it in your `srnd.ini`) or you can [configure your newsreader](extras/configure-newsreader.md).


## Bugs and issues

*PLEASE* report any bugs you find while building, setting-up or using NNTPChan on the [GitHub issue tracker](https://github.com/majestrate/nntpchan/issues), the [issue tracker on tor](http://git.psii2pdloxelodts.onion/psi/nntpchan/), the [issue tracker on i2p](http://git.psi.i2p/psi/nntpchan/) or on the [GitGud issue tracker](https://gitgud.io/jeff/nntpchan/issues) so that the probelms can be resolved or discussed.

## Clients

NNTP (confirmed working):

* Thunderbird

Web:

* [Yukko](https://github.com/faissaloo/Yukko): ncurses based nntpchan web ui reader

## History

* started in mid 2013 on anonet

This is a graph of the post flow of the `overchan.test` newsgroup over 4 years, quite a big network.

(thnx anon who made this btw)

![network topology of 4 years](topology.png "changolia")

[source code for map generation](https://github.com/nilesr/nntpchan-mapper)

## Acknowledgements

* [Deavmi](https://deavmi.carteronline.net/) - Making the documentation beautiful.
