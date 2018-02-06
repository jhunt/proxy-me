Manage Your HTTP(S) Proxy Environment Variables
===============================================

Managing HTTP and HTTPS proxy, and the proxy whitelist, can be
tedious.  Let `proxy-me` make that better for you!

The `proxy-me` utility will output the (Bourne-compatible) shell
commands to update your environment:

```
$ proxy-me --http  proxy.example.com:3128 \
           --https proxy-ssl.example.com:3128 \
           --no-proxy .example.com

echo "Setting Proxy Settings (94dcfd684ccceed3)";
export http_proxy="http://proxy.example.com:3128";
export https_proxy="http://proxy-ssl.example.com:3128";
export no_proxy=".example.com";
```

All you have to do is `eval` it!

```
$ eval $(proxy-me --http  proxy.example.com:3128 \
                  --https proxy-ssl.example.com:3128 \
                  --no-proxy .example.com)

Setting Proxy Settings (94dcfd684ccceed3)
```

And now your proxy environment is set!

That's cool, but `proxy-me` can do a lot more to make life easier.

Normally, the same proxy server is used for both TLS traffic and
unencrypted traffic:

```
$ proxy-me --http proxy.example.com:3128 \
           --no-proxy .example.com

echo "Setting Proxy Settings (94dcfd684ccceed3)";
export http_proxy="http://proxy.example.com:3128";
export https_proxy="http://proxy-ssl.example.com:3128";
export no_proxy=".example.com";
```

It gets even more interesting when you through _CIDR notation_ and
_IP ranges_ at `--no-proxy` (which you can evaluate as `-n`):

Here's a configuration that will whitelist all the hosts in
10.0.241.0/26:

```
$ proxy-me --http proxy.example.com:3128 \
           -n 10.0.241.0/26

echo "Setting Proxy Settings (e8dc3aeaaf8594a2)";
export http_proxy="http://proxy.example.com:3128";
export https_proxy="http://proxy.example.com:3128";
export no_proxy="10.0.241.1,10.0.241.2,10.0.241.3,10.0.241.4,10.0.241.5,10.0.241.6,10.0.241.7,10.0.241.8,10.0.241.9,10.0.241.10,10.0.241.11,10.0.241.12,10.0.241.13,10.0.241.14,10.0.241.15,10.0.241.16,10.0.241.17,10.0.241.18,10.0.241.19,10.0.241.20,10.0.241.21,10.0.241.22,10.0.241.23,10.0.241.24,10.0.241.25,10.0.241.26,10.0.241.27,10.0.241.28,10.0.241.29,10.0.241.30,10.0.241.31,10.0.241.32,10.0.241.33,10.0.241.34,10.0.241.35,10.0.241.36,10.0.241.37,10.0.241.38,10.0.241.39,10.0.241.40,10.0.241.41,10.0.241.42,10.0.241.43,10.0.241.44,10.0.241.45,10.0.241.46,10.0.241.47,10.0.241.48,10.0.241.49,10.0.241.50,10.0.241.51,10.0.241.52,10.0.241.53,10.0.241.54,10.0.241.55,10.0.241.56,10.0.241.57,10.0.241.58,10.0.241.59,10.0.241.60,10.0.241.61,10.0.241.62";
```

I certainly wouldn't want to have to curate that `no_proxy` list
by hand!

Here's a configuration that whitelists the first 5 IP address of
the 192.168.0.0/24 network:

```
$ proxy-me --http proxy.example.com:3128 \
           -n 192.168.0.1-192.168.0.5

echo "Setting Proxy Settings (58dfdebe2e298d79)";
export http_proxy="http://proxy.example.com:3128";
export https_proxy="http://proxy.example.com:3128";
export no_proxy="192.168.0.1,192.168.0.2,192.168.0.3,192.168.0.4,192.168.0.5";
```

You can even specify `--no-proxy` multiple times:

```
$ proxy-me --http proxy.example.com:3128 \
           -n 10.0.0.1-10.0.0.5 \
		   -n 10.200.15.16/30 \
		   -n .example.com

echo "Setting Proxy Settings (3a43aa7043496af9)";
export http_proxy="http://proxy.example.com:3128";
export https_proxy="http://proxy.example.com:3128";
export no_proxy="10.0.0.1,10.0.0.2,10.0.0.3,10.0.0.4,10.0.0.5,10.200.15.17,10.200.15.18,.example.com";
```
