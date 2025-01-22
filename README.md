
![le ebin logo](nntpchan.png "ebin logo")

![MIT License](https://img.shields.io/github/license/majestrate/nntpchan.svg)
![Logo is ebin](https://img.shields.io/badge/logo-ebin-brightgreen.svg)


**NNTPChan** (previously known as overchan) is a decentralized imageboard that uses the [NNTP protocol](https://en.wikipedia.org/wiki/Network_News_Transfer_Protocol) (network-news transfer protocol) to synchronize content between many different servers. It utilizes cryptographically signed posts to perform optional/opt-in decentralized moderation.

## Getting started


[This](doc) is a step-by-step guide for getting up-and-running with NNTPChan as well as documentation for developers who want to either work on NNTPChan directly or use NNTPChan in their aplications with the API.

## Enable Some Modules
```
export GO111MODULE=off
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

## now its time to create the database in postgres
```
CREATE DATABASE nntpchan;
CREATE USER username WITH PASSWORD 'CHANGEME';
GRANT ALL PRIVILEGES ON DATABASE nntpchan TO username;
```


TL;DR edition:


    $ sudo apt update
    $ sudo apt install --no-install-recommends install imagemagick ffmpeg sox build-essential git ca-certificates postgresql postgresql-client golang
    $ git clone https://github.com/majestrate/nntpchan
    $ cd nntpchan
    $ make
    $ SRND_INSTALLER=0 ./srndv2 setup 

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
