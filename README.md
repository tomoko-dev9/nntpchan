![le ebin logo](nntpchan.png "ebin logo")
![MIT License](https://img.shields.io/github/license/majestrate/nntpchan.svg)
![Logo is ebin](https://img.shields.io/badge/logo-ebin-brightgreen.svg)

**NNTPChan** (previously known as overchan) is a decentralized imageboard that uses the [NNTP protocol](https://en.wikipedia.org/wiki/Network_News_Transfer_Protocol) (network-news transfer protocol) to synchronize content between many different servers. It utilizes cryptographically signed posts to perform optional/opt-in decentralized moderation.

## Getting started — Ubuntu 24.04 installation guide

Tested and working on Ubuntu 24.04.

### Step 1: Install dependencies

```bash
sudo apt-get update
sudo apt-get --no-install-recommends install -y \
    imagemagick ffmpeg sox build-essential git ca-certificates \
    postgresql postgresql-client golang
```

### Step 2: Install Go 1.23+

Ubuntu 24.04 ships Go 1.22 which is too old. Install 1.23+ manually:

```bash
curl -L -o /tmp/go1.23.0.tar.gz https://dl.google.com/go/go1.23.0.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf /tmp/go1.23.0.tar.gz
export PATH=/usr/local/go/bin:$PATH
go version  # should show go1.23.x
```

Add to your shell profile to persist:

```bash
echo 'export PATH=/usr/local/go/bin:$PATH' >> ~/.bashrc
```

### Step 3: Get the NNTPChan source

```bash
git clone https://github.com/tomoko-dev9/nntpchan
cd nntpchan
```

### Step 4: Build minify

The Makefile uses old `go get` syntax. Install minify manually:

```bash
GONOSUMDB=* GOPROXY=direct GOPATH=/root/nntpchan/go go install github.com/tdewolff/minify/v2/cmd/minify@latest
```

### Step 5: Fix the Makefile for modern Go

```bash
sed -i 's|GOPATH=$(REPO)|GO111MODULE=off GOPATH=/tmp/srnd-gopath|g' \
    /root/nntpchan/contrib/backends/srndv2/Makefile
```

Set up the GOPATH symlink:

```bash
mkdir -p /tmp/srnd-gopath/src
ln -s /root/nntpchan/contrib/backends/srndv2/src/srnd /tmp/srnd-gopath/src/srnd
```

### Step 6: Compile

```bash
make
```

If the srndv2 step fails, build it manually:

```bash
cd /root/nntpchan/contrib/backends/srndv2
GO111MODULE=off GOPATH=/tmp/srnd-gopath GOROOT=/usr/local/go /usr/local/go/bin/go build -v -o /root/nntpchan/srndv2 .
```

### Step 7: Build the JavaScript bundle

```bash
curl -o /tmp/jquery.js https://cdnjs.cloudflare.com/ajax/libs/jquery/3.7.1/jquery.min.js

cat /tmp/jquery.js \
    /root/nntpchan/contrib/js/entry.js \
    /root/nntpchan/contrib/js/nntpchan/local_storage.js \
    /root/nntpchan/contrib/js/nntpchan/api.js \
    /root/nntpchan/contrib/js/nntpchan/banner.js \
    /root/nntpchan/contrib/js/nntpchan/theme.js \
    /root/nntpchan/contrib/js/nntpchan/expand-image.js \
    /root/nntpchan/contrib/js/nntpchan/expand-video.js \
    /root/nntpchan/contrib/js/nntpchan/hide-post.js \
    /root/nntpchan/contrib/js/nntpchan/post-reply.js \
    /root/nntpchan/contrib/js/nntpchan/reply.js \
    /root/nntpchan/contrib/js/nntpchan/report.js \
    /root/nntpchan/contrib/js/nntpchan/captcha-reload.js \
    /root/nntpchan/contrib/js/nntpchan/crypto.js \
    /root/nntpchan/contrib/js/nntpchan/livechan.js \
    > /root/nntpchan/contrib/static/nntpchan.js
```

### Step 8: Set up PostgreSQL

```bash
sudo -u postgres psql -c "CREATE DATABASE root;"
sudo -u postgres psql -c "CREATE USER root WITH PASSWORD 'root';"
sudo -u postgres psql -c "GRANT ALL PRIVILEGES ON DATABASE root TO root;"
sudo -u postgres psql -d root -c "GRANT ALL ON SCHEMA public TO root;"
sudo -u postgres psql -d root -c "ALTER SCHEMA public OWNER TO root;"
```

> **Note:** Default database, user, and password are all `root`. Default port is `5432`.

### Step 9: Initial setup

```bash
cd /root/nntpchan
./srndv2 setup
```

Edit `srnd.ini` as needed. To disable captcha add `rapeme=omgyesplz` under `[frontend]`. To enable captcha only for new threads (not replies), leave it as `rapeme=no`.

### Step 10: Generate admin keypair

```bash
./srndv2 tool keygen
```

Save the secret key securely. Add the public key as admin:

```bash
./srndv2 tool mod add YOUR_PUBLIC_KEY
```

To post as admin, use your secret key in the name field:

```
Admin#YOUR_SECRET_KEY
```

To show admin styling on posts, add this to your theme CSS (e.g. `krane.css`):

```css
[data-pubkey="YOUR_PUBLIC_KEY"] {
    background-image: url('/static/admin.png');
    background-size: 25%;
    background-repeat: repeat;
}
```

### Step 11: Run as a systemd service

```bash
cat > /etc/systemd/system/nntpchan.service << 'SERVICE'
[Unit]
Description=nntpchan srndv2
After=network.target postgresql.service

[Service]
Type=simple
User=root
WorkingDirectory=/root/nntpchan
ExecStart=/root/nntpchan/srndv2 run
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
SERVICE

systemctl daemon-reload
systemctl enable nntpchan
systemctl start nntpchan
```

The web interface is available at `http://127.0.0.1:18000` by default (configurable in `srnd.ini`).

### Step 12: Set up .onion address (optional)

```bash
apt-get install -y tor
cat >> /etc/tor/torrc << 'TOR'
HiddenServiceDir /var/lib/tor/nntpchan/
HiddenServicePort 80 127.0.0.1:80
TOR
systemctl restart tor
cat /var/lib/tor/nntpchan/hostname
```

---

## What was fixed

- Fixed compilation for modern Go (1.23+)
- Fixed old-style GOPATH builds in the Makefile
- Added posts-per-day graph (overcock graph) to front page
- Added board stats table (Posts this Hour, Posts Today, Total)
- Fixed date offset bug in posts graph
- Built and fixed the JavaScript bundle (livechan, banners, post reply, etc.)
- Captcha only required for new threads, not replies
- Admin posts highlighted with admin.png via pubkey CSS
- Fixed pubkey not loading correctly in thread view
- Fixed PostgreSQL schema permissions for Ubuntu 24.04

---

## Support

[Discord](https://discord.gg)

## Bugs and issues

Please report bugs on the [GitHub issue tracker](https://github.com/tomoko-dev9/nntpchan/issues).

## Clients

NNTP (confirmed working):
* Thunderbird

Web:
* [Yukko](https://github.com/faissaloo/Yukko) — ncurses-based nntpchan web UI reader

## History

* Started in mid 2013 on anonet

![network topology of 4 years](topology.png "changolia")

[source code for map generation](https://github.com/nilesr/nntpchan-mapper)

## Acknowledgements

* [Deavmi](https://deavmi.carteronline.net/) — Making the documentation beautiful.
