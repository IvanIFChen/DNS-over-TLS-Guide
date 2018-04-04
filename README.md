# DNS-over-TLS-Guide
This is a guide to use the new cloudflare's 1.1.1.1 DNS resolver on mac, using DNS over TLS on standard port 853.

**Note:** This is different from the guide on https://1.1.1.1/, simply setting dns server does not provide TLS security for the initial request to the DNS resolver (so your roommates using your wifi can still see what you are browsing!). In order to have the best privacy out of cloudflare's 1.1.1.1, we have to send the request over port 853.

## Install Stubby
```
brew install stubby
```

## Setup Stubby to use 1.1.1.1
open the config file in
```
/usr/local/etc/stubby/stubby.yml
```
replace everything in the `upstream_recursive_servers:` section with just:
```
  - address_data: 1.1.1.1
    tls_auth_name: "cloudflare-dns.com"
  - address_data: 2606:4700:4700::1111
    tls_auth_name: "cloudflare-dns.com"
```

## Setup all upstream DNS traffic to use Stubby
Settings -> Network -> Advanced... -> DNS -> DNS Servers, click on `+` and add `127.0.0.1`.

Start the Stubby daemon and that's it!
```
sudo brew services start stubby
```

## To revert changes
Settings -> Network -> Advanced... -> DNS -> DNS Servers, click on `-` then 
```
sudo brew services stop stubby
```

### Source
https://dnsprivacy.org/wiki/pages/viewpage.action?pageId=3145812

https://github.com/getdnsapi/stubby

https://developers.cloudflare.com/1.1.1.1/dns-over-tls/

