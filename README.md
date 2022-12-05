`# mkdir /etc/wireguard`

`# pkg_add wireguard-tools`

`# pkg_add nano`

`# wg genkey > private_key`

`# wg pubkey < private_key > public_key`
 
`# nano /etc/wireguard/server.conf`
```
[Interface]
PrivateKey = SERVER PRIVATE KEY
ListenPort = 51820
[Peer]
PublicKey = CLIENT PUBLIC KEY
AllowedIPs = 192.168.0.0/16
```
 
`# nano /etc/hostname.wg0`
```
inet 10.0.0.1 255.255.255.0
!/usr/local/bin/wg setconf wg0 /etc/wireguard/server.conf
```

`# sh /etc/netstart wg0`

`# nano client.conf`
```
[Interface]
PrivateKey = CLIENT PRIVATE KEY
Address = 10.0.0.2/32
DNS = 192.168.1.1
[Peer]
PublicKey = SERVER PUBLIC KEY
AllowedIPs = 0.0.0.0/0, ::/0
Endpoint = server ip:51820
```

`# cp client.conf /etc/wireguard/client.conf`

`# wg-quick up client`

`# ping www.google.com`
 
`# pkg_add tor`

`# nano /etc/pf.conf`
```
pass in on egress proto tcp from 192.168.0.0/16 to any rdr-to 127.0.0.1 port 9040
pass in on egress proto udp from 192.168.0.0/16 to any port 53 rdr-to 127.0.0.1 port 5353
```
 
`# pfctl -f  /etc/pf.conf`

`# nano /etc/tor/torrc`
```
TransPort 9040
DNSPort 5353
```
 
`# rcctl enable tor`
