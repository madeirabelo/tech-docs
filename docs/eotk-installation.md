---
layout: default
title: EOTK - How to Install v1
nav_order: 101
---

{: .no_toc }

{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

# [](#header-1)EOTK (the Enterprise Onion Toolkit)

## [](#header-2)Installation Macro Steps

1. Generate a "vanity" .onion domain name using mkp2240.

2. Install and configure EOTK on a remote web server.

3. Install trusted TLS certificates from HARICA into EOTK.

## [](#header-2)Generate a "vanity" .onion domain name using mkp2240

### [](#header-3)Using mkp224o to Generate “Vanity” Onion Domain Names

Generate these vanity onion domain names using a tool called [mkp224o](https://github.com/cathugger/mkp224o) (Github Link).
Note that mkp224o program is essentially an "onion miner".

Example of Vanity addresses

BBC NEWS [https://www.bbcnewsd73hkzno2ini43t4gblxvycyac5aw4gnv7t2rccijh7745uqd.onion/](https://www.bbcnewsd73hkzno2ini43t4gblxvycyac5aw4gnv7t2rccijh7745uqd.onion/)

Twitter [http://twitter3e4tixl4xyajtrzo62zg5vztmjuricljdp2c5kshju4avyoid.onion/](http://twitter3e4tixl4xyajtrzo62zg5vztmjuricljdp2c5kshju4avyoid.onion/)

NY Times [http://nytimesn7cgmftshazwhfgzm37qxb44r64ytbb2dj3x62d2lljsciiyd.onion/](http://nytimesn7cgmftshazwhfgzm37qxb44r64ytbb2dj3x62d2lljsciiyd.onion/)

[Link to another page](another-page).

```
# Downloading mkp224o
> git clone https://github.com/cathugger/mkp224o.git

# Tool to compile mkp224o
> sudo apt install gcc libsodium-dev make autoconf

# Generate a ./configure autoconf script
> cd mkp224o<br/><br/>> ./autogen.s

# Configuration options to generate onions faster
# OPTIMISATION.txti
# https://github.com/cathugger/mkp224o/blob/master/OPTIMISATION.txt

# Configuring mkp224o with optimal feature flags for a modern CPU
> ./configure --enable-amd64-51-30k --enable-intfilter

# Compile the program
> make
> ./mkp224o -h

# Create a ./my-filters.txt file with a list of "filters" that we want 
# our onion domain to be. Tips when mining Onion Addresses 
# https://github.com/alecmuffett/eotk/blob/master/docs.d/TIPS-FOR-MINING-ONIONS.md
> nano ./my-filters.txt

# example of a filter file
# hongonion
# shenhong
# hongblog
# shenblog
# shensite
# hongio
# shenio

# The directory ./candidates/ which will contain all the matching domains
# PASSPHRASE should be longer than 64 random characters
> ./mkp224o -s --checkpoint checkpoint.txt -d candidates -f my-filters.txt -B -p PASSPHRASE


# For a 75% chance to "mine" a single 8-character vanity onion domain 
# name, you will need approximately 31 hours on a modern Intel i7 CPU. 
# That same 8-character vanity onion domain name will take less than 
# 5 hours on a high-end AMD EPYC 7003 server CPU with 24 cores. 
# That same powerful server CPU will take more than 4 days to mine a 
# 9-character onion
# More than 191 days to mine a 10-character onion!
```

## [](#header-2)Install and configure EOTK on a remote web server

### [](#header-3)Introduction to EOTK: The Enterprise Onion Toolkit

EOTK serves as a translator which allows Tor Network users to visit a site as an onion site, and for the site to respond as an onion site.

![](../eotk-installation-images/1.png)

EOTK sits between the application and Nginx, dynamically rewriting requests that cross the Tor Network.
EOTK is effectively a modified Nginx reverse proxy, which "translates" requests between the Tor Network, and your application's regular reverse proxy.
The connection between EOTK and your Web Application is not direct, but rather EOTK is a layer "on top of" your regular Nginx reverse proxy.

### Installing EOTK on Ubuntu (20.04)

It is possible to install EOTK in: 
centos-8.2.2004; freebsd-12.1; macos-catalina; macos-mojave; raspbian-stretch
ubuntu-18.04; ubuntu-20.04

```
# Place yourself in the home directory
> cd ~

# Clone the EOTK repository onto your remote webserver
> git clone https://github.com/alecmuffett/eotk.git
> cd ./eotk
> ./opt.d/build-ubuntu-20.04.sh

> tree -L 3
.
│   # Directories
├── demo.d
├── docs.d
├── lib.d
├── opt.d
├── projects.d
├── secrets.d
├── templates.d
├── tools.d
│   ├── openresty.d
│   ├── tor -> /root/eotk/opt.d/tor.d/bin/tor
│   └── tor.d
│
│   # Executables
├── eotk
├── eotk-housekeeping.sh
├── eotk-init.sh
│
│   # Other files
├── LICENSE
├── Makefile
└── README.md

# Example site configuration files for demonstration. 
# The wikipedia.conf file is a good source of inspiration
demo.d

# Documentation
docs.d

# Shell and Perl scripts used by the eotk binary.
lib.d

# Contains the Tor daemon, and OpenResty binaries.
opt.d

# This is equivalent to your /etc/nginx/sites-available/ folder. 
# All of your site configuration, and TLS keys will be held here.
projects.d

# This directory exclusively holds the public-key pairs for your 
# .onion domain. 
secrets.d

# Using in the building process of the eotk binary, as well as 
# to generate project configuration files.
templates.d

# Contains tools.
tools.d

# Every website you choose to make available as an onion site
# will have its own folder within projects.d
projects.d
├── website1.com.d # Includes all subdomains of website1.com
├── website2.net.d
└── website3.org.d
```

#### EOTK Binary and Command Reference

```
# Generates a Nginx configuration file from a given EOTK
# configuration file.
./eotk config <website.com.conf>

# Checks the configuration syntax of a given project.
# Analogous to nginx -t.
./eotk syntax <project>

# Reloads the configuration file of a given project.
# Analogous to systemctl reload nginx.
./eotk nxreload <project>

# Displays the status of a given project. To list the status
# of all projects, call with -a.
./eotk status <project>

# Starts a given project. Analogous to linking a Nginx file
# from /etc/nginx/sites-available to /etc/nginx/sites-enabled
./eotk start <project>

# Restarts a given project. Analogous to systemctl restart nginx.
./eotk restart <project>

# Stops a given project. Analogous to systemctl stop nginx.
./eotk stop <project>

# Lists all projects.
./eotk projects
```

#### Creating EOTK Configuration Files for Projects

```
# Create a folder within 
# ~/eotk/projects.d/, such as ~/eotk/projects.d/example.com.d
> set project domain_name

# In the mining machine
stop mkp224o (Ctrl + C) 
# Inside the candidates/ directory there are some folders like
# domain_name.onion/
# Select the best domain_name for the project
# Inside there are the following files:
# hostname (contains the .onion domain)
# hs_ed25519_public_key (public and private key pair)
# hs_ed25519_secret_key
```

#### Configuring Hardmaps for EOTK Projects

```
> cd ~/eotk/
> cat domain_name.conf
set project domain_name
hardmap fshy2rdkdxzy42iurxqdj4opmm4mbpohc57mqc7dolzwjtbbmm76u6iy domain_name.com stories
```

#### Creating regular Onion Address

```
> nano your_project.tconf

# Edit “your_project.tconf”
# exit with “Ctrl S” “Ctrl X”
set project your_project
hardmap %NEW_V3_ONION% your_website.com

> ./eotk config your_project.tconf
> rm your_project.tconf
> more your_project.conf
# your_project.conf will have the onion_address to use it the TOR browser
set project your_project
hardmap onion_address your_website.com

# Try the onion_address.onion in TOR
> ./eotk start your_project
```

#### Activating, Deactivating, and Reloading EOTK configuration

```
> ./eotk config domain_name.conf
# EOTK will create the directory in 
# ~/eotk/projects.d/domain_name.d/eotk which will contain all our 
# project-specific configuration, and startup/shutdown scripts.
# EOTK will instantiate a project-specific Nginx instance, with
# an autogenerated Nginx configuration file at
# ~/eotk/projects.d/domain_name.d/nginx.conf. This configuration file
# will contain the OpenResty rewriting rules that "translate" our
# clearnet site to an onion site, and vice versa.
# EOTK will configure the tor daemon service with the provided
# onion domain key-files, to create a hardmap to our 
# EOTK Nginx instance. It will autogenerate a Tor configuration file at
# ~/eotk/projects.d/domain_name.d/tor.conf.
# Incoming Tor Network requests for our onion site will be handed
# over to the EOTK Nginx instance, translated, and passed through to
# our regular Nginx instance (and vice versa).

# Start service
> ./eotk start domain_name.com
```

#### Activating EOTK as a Systemd Service and Starting Automatically on Boot

```
# This will create the eotk-init.sh and eotk-housekeeping.sh files
# in the same location as your ./eotk binary. The eotk-init.sh file
# is a System V-style init script which we can move to the
# /etc/init.d directory, and then convert it automatically into
# a Systemd-style service unit. 
> ./eotk make-scripts

> cp eotk-init.sh /etc/init.d
> update-rc.d eotk-init.sh defaults

# EOTK installation will autostart on boot, and always be available.
systemctl start  eotk
systemctl status eotk
```

### Install trusted TLS certificates from HARICA into EOTK.

#### Purchasing an Onion TLS Certificate from HARICA

| Choose "Server Certificates" in [https://www.harica.gr](https://www.harica.gr) |
|![](../eotk-installation-images/2.png)  |
|                                         |
| Choose a name for the Key, and give the onion address (ending with .onion)(with a subdomain *. for all the possible subdomains)                      |
|![](../eotk-installation-images/3.png)   |
|                                         |
| 30€ per year;     150€ per year with * subdomain  |
|![](../eotk-installation-images/4.png)   |
|                                         |
|![](../eotk-installation-images/5.png)   |
|                                         |
|![](../eotk-installation-images/6.png)   |

```
> apt install ruby build-essential
> git clone --recurse-submodules https://github.com/HARICA-official/onion-csr.git
> cd onion-csr
> gem install ffi
> gcc -shared -o libed25519.so -fPIC ed25519/src/*.c
> ./onion-csr.rb -n <NONCE> -d ~/eotk/projects.d/domain_name.d/fshy2rdkdxzy42iurxqd-v3.d/
# The utility should immediately output the onion CSR to the screen. 
```

| Copy the onion CSR and submit it to HARICA. When pasting the onion CSR, make sure that there are no extra newlines at the end.|
|![](../eotk-installation-images/7.png)    |
|![](../eotk-installation-images/8.png)    |

#### Installing Signed TLS Certificates into EOTK

| Get the certificate.pem file: |
| Go to the Dashboard           |
|![](../eotk-installation-images/9.png) |
|                               |
| Choose Download               |
|![](../eotk-installation-images/10.png)|

```
> rsync -a certificate.pem username@remote:/path/to/destination
> rsync -a privateKey.pem username@remote:/path/to/destination
> cp certificate.pem eotk/projects.d/domain_name.d/ssl.d/fshy2rdkdxzy42iurxqd-v3.cert
> openssl ec -in privateKey.pem -out unlockedKey.pem
# Enter private key passphrase
> cp unlockedKey.pem eotk/projects.d/domain_name.d/ssl.d/fshy2rdkdxzy42iurxqd-v3.pem
> ./eotk nxreload domain_name
```

---

# OPTIMISATION.txt

```
This document describes configuration options which may help one to generate onions faster.
First of all, default configuration options are tuned for portability, and may be a bit suboptimal.
User is expected to pick optimal settings depending on hardware mkp224o will run on and ammount of filters.


ED25519 implementations:
mkp224o includes multiple implementations of ed25519 code, tuned for different processors.
Implementation is selected at configuration time, when running `./configure` script.
If one already configured/compiled code and wants to change options, just re-run
`./configure` and also run `make clean` to clear compiled files, if any.
Note that options and CFLAGS/LDFLAGS settings won't carry over from previous configure run,
so you have to include options you've previously configured, if you want them to remain.
At the time of writing, these implementations are present:
+----------------+-----------------------+----------------------------------------------------------+
| implementation | enable flag           | notes                                                    |
|----------------+-----------------------+----------------------------------------------------------+
| ref10          | --enable-ref10        | SUPERCOP' ref10, pure C, very portable, previous default |
| amd64-51-30k   | --enable-amd64-51-30k | SUPERCOP' amd64-51-30k, only works on x86_64             |
| amd64-64-24k   | --enable-amd64-64-24k | SUPERCOP' amd64-64-24k, only works on x86_64             |
| ed25519-donna  | --enable-donna        | based on amd64-51-30k, C, portable, current default      |
| ed25519-donna  | --enable-donna-sse2   | uses SSE2, needs x86 architecture                        |
+----------------+-----------------------+----------------------------------------------------------+
When to use what:
 - on 32-bit x86 architecture "--enable-donna" will probably be fastest, but one should try
   using "--enable-donna-sse2" too
 - on 64-bit x86 architecture, it really depends on your processor; "--enable-amd64-51-30k"
   worked best for me, but you should really benchmark on your own machine
 - on ARM "--enable-donna" will probably work best
 - otherwise you should benchmark, but "--enable-donna" will probably win

Please note, that these recomendations may become out of date if more implementations
are added in the future; use `./configure --help` to obtain all available options.
When in doubt, benchmark.


Onion filtering settings:
mkp224o supports multiple algorithms and data types for filtering.
Depending on your use case, picking right settings may increase performance.
At the time of writing, mkp224o supports 2 algorithms for filter searching:
sequential and binary search. Sequential search is default, and will probably
be faster with small ammount of filters. If you have lots of filters (lets say >100),
then picking binary search algorithm is the right way.
mkp224o also supports multiple filter types: filters can be represented as integers
instead of being binary strings, and that can allow better compiler's optimizations
and faster code (dealing with fixed-size integers instead of variable-length strings is simpler).
On the other hand, fixed size integers limit length of filters, therefore
binary strings are used by default.

Current options, at the time of writing:
  --enable-binsearch      enable binary search algoritm; MUCH faster if there
                          are a lot of filters. by default, if this isn't enabled,
                          sequential search is used

  --enable-intfilter[=(32|64|128|native)]
                          use integers of specific size (in bits) [default=64]
                          for filtering. faster but limits filter length to:
                          6 for 32-bit, 12 for 64-bit, 24 for 128-bit. by default,
                          if this option is not enabled, binary strings are used,
                          which are slower, but not limited in length.

  --enable-binfilterlen=VAL
                          set binary string filter length (if you don't use intfilter).
                          default is 32 (bytes), which is maximum key length.
                          this may be useful for decreasing memory usage if you
                          have a lot of short filters, but then using intfilter
                          may be better idea.

  --enable-besort         force intfilter binsearch case to use big endian
                          sorting and not omit masks from filters; useful if
                          your filters aren't of same length.
                          let me elaborate on this one.
                          by default, when binary search algorithm is used with integer
                          filters, we actually omit filter masks and use global mask variable,
                          because otherwise we couldn't reliably use integer comparison operations
                          combined with per-filter masks, as sorting order there is unclear.
                          this is because majority of processors we work with are little-endian.
                          therefore, to achieve proper filtering in case where filters
                          aren't of same length, we flatten them by inserting more filters.
                          binary searching should balance increased overhead here to some extent,
                          but this is definitely not optimal and can bloat filtering table
                          very heavily in some cases (for example if there exists say 1-char filter
                          and 8-char filter, it will try to flatten 1-char filterto 8 chars
                          and add 32*32*32*32*32*32*32 filters to table which isn't really good).
                          this option makes us use big-endian way of integer comparison, which isn't
                          native for current little-endian processors but should still work much better
                          than binary strings. we also then are able to have proper per-filter masks,
                          and don't do stupid flattening tricks which may backfire.

                          TL;DR: its quite good idea to use this if you do "--enable-binsearch --enable-intfilter"
                          and have some random filters which may have different length.


Batch mode:
mkp224o now includes experimental key generation mode which performs certain operations in batches,
and is around 15 times faster than current default.
It is currently experimental, and is activated by -B run-time flag.
Batched element count is configured by --enable-batchnum=number option at configure time,
increasing or decreasing it may make batch mode faster or slower, depending on hardware.


Benchmarking:
It's always good idea to see if your settings give you desired effect.
There currently isn't any automated way to benchmark different configuration options, but it's pretty simple to do by hand.
For example:
# prepare configuration script
./autogen.sh
# try default configuration
./configure
# compile
make
# benchmark implementation speed
./mkp224o -s -d res1 neko
# wait for a while, copy statistics to some text editor
^C # stop experiment when you've collected enough data
# try with different settings now
./configure --enable-amd64-64-24k --enable-intfilter
# clean old compiled files
make clean
# recompile
make
# benchmark again
./mkp224o -s -d res2 neko
# wait for a while, copy statistics to some text editor
^C # stop experiment when you've collected enough data
# configure again, make clean, make, run test again.......
# until you've got enough data to make decisions

when benchmarking filtering settings, remember to actually use filter files you're going to work with.


What options I use:
For my lappy with old-ish i5 I do `./configure --enable-amd64-51-30k --enable-intfilter` incase I want single onion,
and `./configure --enable-amd64-51-30k --enable-intfilter --enable-binsearch --enable-besort` when playing with dictionaries.
For my raspberry pi 2, `./configure --enable-donna --enable-intfilter`
(and also +=" --enable-binsearch --enable-besort" for dictionaries).
```

# [ii]wikipedia.tconf

```
> less ~/eotk/demo.d/wikipedia.tconf
# -*- conf -*-
# eotk (c) 2017 Alec Muffett

# CSVs of canonical domains (eg: email) to preserve (todo: more here?)
# nb: you must explicitly list all domains that are of preservation;
# "foo.com" & "www.foo.com" are treated as separate, for this purpose
set preserve_csv \
    tld-wp,wikipedia\\.org,i,wikipedia.org \
    tld-wm,wikimedia\\.org,i,wikimedia.org

# FIX THIS TO USE A LOCAL RESOLVER, BECAUSE PERFORMANCE
set nginx_resolver \
    8.8.8.8 \
    8.8.4.4 \
    ipv6=off

# cache persistence & size; sized for RaspberryPi (256m)
set nginx_cache_seconds 60
set nginx_cache_size 256m
set nginx_tmpfile_size 64m

# proof-of-concept: let's make this read-only:
set suppress_methods_except_get 1

# proof-of-concept: block access to some hosts
set block_host_re \
    ^(login|donate)\\.

# proof-of-concept: block access to some paths
set block_path_re \
    /User: \
    /Special:(UserLogin|(Create|Merge)Account|RenameRequest)\\b

# proof-of-concept: block requests where parameters have certain values
set block_param_re \
    title,^User: \
    title,^Special:(UserLogin|(Create|Merge)Account|RenameRequest)\\b

# proof-of-concept: blacklist requests to some paths
set path_blacklist_re \
    ^\\. \
    ^\\w+\\.php$ \
    \\.(sql|gz|tgz|zip|bz2)$ \
    ^server-status$

# proof-of-concept: whitelist reasonable user-agents (anything else => ded)
set user_agent_whitelist_re \
    ^Mozilla.*Gecko

# suggestion: you might want to investigate "no_cache_content_type" or
# "no_cache_host" if you want limitations on caching...

# demo: CSV list to implement ownership proof URIs for EV SSL issuance
# set hardcoded_endpoint_csv \
#     ^/proof/foo/?$,"FOOPROOF" \
#     ^/proof/bar/?$,"BARPROOF"

# demo: magic cookie-issuing URL to restrict access until ready to launch
# set cookie_lock /open-sesame

# index of other onion sites ("what happens in onion, should stay in onion")
foreignmap facebookwkhpilnemxj7asaniu7vnjjbiltxjqhye3mhbshg7kx5tfyd facebook.com

# the Wikimedia Foundation have lots of sites
set project wikipedia
hardmap %NEW_V3_ONION% mediawiki.org
hardmap %NEW_V3_ONION% wikidata.org
hardmap %NEW_V3_ONION% wikimedia.org
hardmap %NEW_V3_ONION% wikimediafoundation.org
# the following have an `m` subdomain
hardmap %NEW_V3_ONION% wikibooks.org m
hardmap %NEW_V3_ONION% wikinews.org m
hardmap %NEW_V3_ONION% wikipedia.org m
hardmap %NEW_V3_ONION% wikiquote.org m
hardmap %NEW_V3_ONION% wikisource.org m
hardmap %NEW_V3_ONION% wikiversity.org m
hardmap %NEW_V3_ONION% wikivoyage.org m
hardmap %NEW_V3_ONION% wiktionary.org m
# nb: by subdomain we mean FOO in en.FOO.wikipedia.org, etc.
hardmap %NEW_V3_ONION% wikidata.org
hardmap %NEW_V3_ONION% wikimedia.org
hardmap %NEW_V3_ONION% wikimediafoundation.org
# the following have an `m` subdomain
hardmap %NEW_V3_ONION% wikibooks.org m
hardmap %NEW_V3_ONION% wikinews.org m
hardmap %NEW_V3_ONION% wikipedia.org m
hardmap %NEW_V3_ONION% wikiquote.org m
hardmap %NEW_V3_ONION% wikisource.org m
hardmap %NEW_V3_ONION% wikiversity.org m
hardmap %NEW_V3_ONION% wikivoyage.org m
hardmap %NEW_V3_ONION% wiktionary.org m
# nb: by subdomain we mean FOO in en.FOO.wikipedia.org, etc.
```

# [iii]

./eotk config domain_name.conf command will generate tor.conf and nginx.conf

# tor.conf

```
> cat ~/eotk/projects.d/domain.d/tor.conf

# -*- conf -*-
# eotk (c) 2017 Alec Muffett

DataDirectory /home/server/eotk/projects.d/domain_name.d
ControlPort unix:/home/server/eotk/projects.d/domain_name.d/tor-control.sock
PidFile /home/server/eotk/projects.d/domain_name.d/tor.pid
Log notice file /home/server/eotk/projects.d/domain_name.d/log.d/tor.log
SafeLogging 1
HeartbeatPeriod 60 minutes
LongLivedPorts 80,443
RunAsDaemon 1

# use single onions, many settings to tweak:
SocksPort 0
HiddenServiceSingleHopMode 1
HiddenServiceNonAnonymousMode 1

# hardmap for: domain_name.com -> fshy2rdkdxzy42iurxqdj4opmm4mbpohc57mqc7dolzwjtbbmm76u6iy.onion
HiddenServiceDir /home/server/eotk/projects.d/_domain_name_.d/fshy2rdkdxzy42iurxqd-v3.d
HiddenServicePort 80 unix:/home/server/eotk/projects.d/domain_name.d/fshy2rdkdxzy42iurxqd-v3.d/port-80.sock
HiddenServicePort 443 unix:/home/server/eotk/projects.d/domain_name.d/fshy2rdkdxzy42iurxqd-v3.d/port-443.sock
HiddenServiceVersion 3
```

X

X

X

X
