```shell
DCFW-1800# show conf

姝ｅ湪鏀堕泦閰嶇疆..
Running configuration:
!
Version 5.4

ip vrouter "trust-vr"
exit
vswitch "vswitch1"
exit
zone "trust"
exit
zone "untrust"
exit
zone "dmz"
exit
zone "l2-trust" l2
exit
zone "l2-untrust" l2
exit
zone "l2-dmz" l2
exit
zone "VPNHub"
exit
zone "HA"
exit
zone "DMZ"
exit      
interface vswitchif1
exit
interface ethernet0/0
exit
interface ethernet0/1
exit
interface ethernet0/2
exit
interface ethernet0/3
exit
interface ethernet0/4
exit
interface ethernet0/5
exit
interface ethernet0/6
exit
interface ethernet0/7
exit
interface ethernet0/8
exit
interface aggregate1
exit
interface aggregate2
exit      
interface loopback1
exit
interface tunnel1
exit
interface aggregate1.21
exit
interface aggregate1.22
exit
interface aggregate1.23
exit
address id 3 "ssl_pool"
exit
address id 4 "net_internet"
exit
address id 5 "net_lan"
exit
address id 6 "Web"
exit
address id 7 "WAI"
exit
qos-profile "pro_app_$in_110640728" app-qos-profile
exit
qos-profile "pro_app_$ou_110640728" app-qos-profile
exit      
aaa-server "local" type local
exit
schedule "work"
  periodic monday tuesday wednesday thursday friday 09:00 to 18:00
exit
contentfilter
  category "鐥呮瘨璧屽崥"
  keyword "鐥呮瘨" simple category "鐥呮瘨璧屽崥" confidence 100
  keyword "璧屽崥" simple category "鐥呮瘨璧屽崥" confidence 100
exit
aaa-server "local" type local
  user "ABC4321"
    password "q9jbZlQCOFWYzF4R+5GZFgU+lBI4"
  exit
exit
admin user "admin"
  password uwlZQwjMo/113AkUVFZZ+mpQgN
  privilege RXW
  access console
  access telnet
  access ssh
  access http
  access https
exit      
logging syslog 20.10.28.10 vrouter "trust-vr" udp 2000 type traffic t
pki trust-domain "trust_domain_default"
  keypair "Default-Key"
  enrollment self
  subject commonName "DCFW-1800"
  subject organization "DigitalChina Networks Limited"
exit
pki trust-domain "trust_domain_ssl_proxy"
  keypair "Default-Key"
  enrollment self
  subject commonName "DCFW-1800"
  subject organization "DigitalChina Networks Limited"
exit
pki trust-domain "network_manager_ca"
  enrollment terminal
exit
address id 3 "ssl_pool"
  range 192.168.10.1 192.168.10.20
exit
address id 4 "net_internet"
  range 202.22.1.3 202.22.1.4
exit
address id 5 "net_lan"
  ip 20.1.61.0/24
  ip 20.1.60.0/24
exit
address id 6 "Web"
  ip 20.10.28.10/32
exit
address id 7 "WAI"
  ip 202.22.1.1/32
exit
zone "trust"
  qos-profile 1st-level input "pro_app_$in_110640728"
  qos-profile 1st-level output "pro_app_$ou_110640728"
  ad icmp-flood
  ad udp-flood
  ad ip-sweep
  ad port-scan
  ad ip-spoofing
  ad winnuke
  ad ip-fragment
  ad land-attack
  ad ip-option
  ad ip-directed-broadcast
  ad tear-drop
  ad ping-of-death
  ad syn-flood
  ad syn-flood destination ip-based
exit
zone "untrust"
  type wan
  ad tear-drop
  ad ip-spoofing
  ad land-attack
  ad ip-option
  ad ip-fragment
  ad ip-directed-broadcast
  ad winnuke
  ad port-scan
  ad syn-flood
  ad icmp-flood
  ad ip-sweep
  ad ping-of-death
  ad udp-flood
exit
zone "l2-untrust" l2
  type wan
exit
zone "DMZ"
  vrouter "trust-vr"
exit      
hostname "DCFW-1800"
language zh_CN
admin host any any
isakmp proposal "psk-md5-des-g2"
  hash md5
  encryption des
exit

isakmp proposal "psk-md5-3des-g2"
  hash md5
exit

isakmp proposal "psk-md5-aes128-g2"
  hash md5
  encryption aes
exit

isakmp proposal "psk-md5-aes256-g2"
  hash md5
  encryption aes-256
exit

isakmp proposal "psk-sha-des-g2"
  encryption des
exit

isakmp proposal "psk-sha-3des-g2"
exit

isakmp proposal "psk-sha-aes128-g2"
  encryption aes
exit

isakmp proposal "psk-sha-aes256-g2"
  encryption aes-256
exit

isakmp proposal "rsa-md5-des-g2"
  authentication rsa-sig
  hash md5
  encryption des
exit

isakmp proposal "rsa-md5-3des-g2"
  authentication rsa-sig
  hash md5
exit
          
isakmp proposal "rsa-md5-aes128-g2"
  authentication rsa-sig
  hash md5
  encryption aes
exit

isakmp proposal "rsa-md5-aes256-g2"
  authentication rsa-sig
  hash md5
  encryption aes-256
exit

isakmp proposal "rsa-sha-des-g2"
  authentication rsa-sig
  encryption des
exit

isakmp proposal "rsa-sha-3des-g2"
  authentication rsa-sig
exit

isakmp proposal "rsa-sha-aes128-g2"
  authentication rsa-sig
  encryption aes
exit

isakmp proposal "rsa-sha-aes256-g2"
  authentication rsa-sig
  encryption aes-256
exit

isakmp proposal "dsa-sha-des-g2"
  authentication dsa-sig
  encryption des
exit

isakmp proposal "dsa-sha-3des-g2"
  authentication dsa-sig
exit

isakmp proposal "dsa-sha-aes128-g2"
  authentication dsa-sig
  encryption aes
exit

isakmp proposal "dsa-sha-aes256-g2"
  authentication dsa-sig
  encryption aes-256
exit

isakmp proposal "p1"
  hash md5
  group 1
exit

isakmp peer "BC"
  isakmp-proposal "p1"
  pre-share "GuRDgwP44ZcnG3xMDVl6MSrolEoBAy"
  peer 203.23.1.2
  interface aggregate1.23
exit

ipsec proposal "esp-md5-des-g2"
  hash md5
  encryption des
  group 2
exit

ipsec proposal "esp-md5-des-g0"
  hash md5
  encryption des
exit      

ipsec proposal "esp-md5-3des-g2"
  hash md5
  encryption 3des
  group 2
exit

ipsec proposal "esp-md5-3des-g0"
  hash md5
  encryption 3des
exit

ipsec proposal "esp-md5-aes128-g2"
  hash md5
  encryption aes
  group 2
exit

ipsec proposal "esp-md5-aes128-g0"
  hash md5
  encryption aes
exit

ipsec proposal "esp-md5-aes256-g2"
  hash md5
  encryption aes-256
  group 2
exit

ipsec proposal "esp-md5-aes256-g0"
  hash md5
  encryption aes-256
exit

ipsec proposal "esp-sha-des-g2"
  hash sha
  encryption des
  group 2
exit

ipsec proposal "esp-sha-des-g0"
  hash sha
  encryption des
exit

ipsec proposal "esp-sha-3des-g2"
  hash sha
  encryption 3des
  group 2
exit

ipsec proposal "esp-sha-3des-g0"
  hash sha
  encryption 3des
exit

ipsec proposal "esp-sha-aes128-g2"
  hash sha
  encryption aes
  group 2
exit

ipsec proposal "esp-sha-aes128-g0"
  hash sha
  encryption aes
exit

ipsec proposal "esp-sha-aes256-g2"
  hash sha
  encryption aes-256
  group 2
exit      

ipsec proposal "esp-sha-aes256-g0"
  hash sha
  encryption aes-256
exit

ipsec proposal "p2"
  hash md5
  encryption 3des
exit

tunnel ipsec "R" auto
  isakmp-peer "BC"
  ipsec-proposal "p2"
  track-event-notify enable
exit
scvpn pool "vlan61"
  address 20.1.60.100 20.1.60.200 netmask 255.255.255.0
  dns 8.8.8.8
exit
scvpn pool "SSL Pool"
  address 192.168.10.1 192.168.10.20 netmask 255.255.255.192
exit
tunnel scvpn "vpn_TO_vlan_61"
  https-port 4455
  pool "SSL Pool"
  split-tunnel-route 0.0.0.0/0 metric 1
  aaa-server "local"
  interface aggregate1.23
exit
dhcp-server pool "vlan60"
  gateway 20.1.60.1
  address  20.1.60.10 20.1.60.100
  dns 8.8.8.8
exit
interface ethernet0/0
  zone  "trust"
  ip address 192.168.1.1 255.255.255.0
  manage ssh
  manage telnet
  manage ping
  manage snmp
  manage http
  manage https
exit
interface ethernet0/1
  aggregate aggregate1
exit      
interface ethernet0/2
  aggregate aggregate1
exit
interface ethernet0/3
  zone  "DMZ"
  ip address 20.10.28.1 255.255.255.0
  manage telnet
  manage ssh
  manage ping
  manage http
  manage https
  manage snmp
  reverse-route prefer
exit
interface ethernet0/4
  aggregate aggregate2
exit
interface ethernet0/5
  aggregate aggregate2
exit
interface ethernet0/8
  zone  "trust"
  ip address 192.168.99.2 255.255.255.0
  manage telnet
  manage ssh
  manage ping
  manage http
  manage https
  manage snmp
  reverse-route prefer
exit
interface aggregate1
  zone  "trust"
  manage ping
  manage http
  manage snmp
  manage telnet
  reverse-route prefer
exit
interface aggregate2
  zone  "trust"
  ip address 20.1.0.14 255.255.255.252
  manage telnet
  manage ping
  manage http
  manage snmp
  ip ospf authentication message-digest
  ip ospf message-digest-key 1 md5 GtIX7KCpy8aI20lpWYBxumuLRWcM
  reverse-route prefer
exit
interface loopback1
  zone  "trust"
  ip address 20.0.0.254 255.255.255.255
  manage telnet
  manage ping
  manage http
  manage snmp
  reverse-route prefer
exit
interface tunnel1
  zone  "untrust"
  ip address 192.168.10.1 255.255.255.192
  manage telnet
  manage ping
  manage http
  manage snmp
  tunnel scvpn "vpn_TO_vlan_61"
  reverse-route prefer
exit
interface aggregate1.21
  zone  "trust"
  ip address 20.1.0.1 255.255.255.252
  manage telnet
  manage ping
  manage http
  manage snmp
  reverse-route prefer
exit
interface aggregate1.22
  zone  "untrust"
  ip address 20.1.1.1 255.255.255.252
  manage ssh
  manage ping
  manage https
  manage snmp
  reverse-route prefer
exit
interface aggregate1.23
  zone  "untrust"
  ip address 202.22.1.1 255.255.255.248
  manage ssh
  manage ping
  manage https
  manage snmp
exit
ip vrouter "trust-vr"
  snatrule id 1 from "20.1.0.14" to "202.22.1.1" service "Any" transi
cky 
  dnatrule id 1 from "Any" to "202.22.1.1" service "HTTPS" trans-to  
  ip route 0.0.0.0/0 202.22.1.2
  ip route 20.1.41.0/24 20.1.1.2
  router ospf
    router-id 20.0.0.254
    default-information originate
    network 20.0.0.254/32 area 0.0.0.0
    network 20.1.0.12/30 area 0.0.0.0
    network 20.10.28.0/24 area 0.0.0.0
    network 20.1.0.0/30 area 0.0.0.0
    network 20.1.1.0/30 area 0.0.0.0
    network 202.22.1.0/29 area 0.0.0.0
  exit
exit
class-map "p2p"
  match application "APP_P2P"
  match application "APP_P2P_STREAM"
exit
class-map "http"
  match application "HTTP"
exit
qos-profile "pro_app_$in_110640728" app-qos-profile
  class "p2p"
    police 2048 conform-action transmit  exceed-action drop 
    match-priority 2
  exit
  class "http"
    police 102400 conform-action transmit  exceed-action drop 
    match-priority 1
  exit
exit
qos-profile "pro_app_$ou_110640728" app-qos-profile
  class "p2p"
    bandwidth 1024
    match-priority 2
  exit
  class "http"
    bandwidth 102400
    match-priority 1
  exit
exit
rule id 1
  action permit
  src-zone "trust"
  dst-zone "untrust"
  src-addr "Any"
  dst-addr "Any"
  service "Any"
exit
rule id 2
  action permit
  src-zone "untrust"
  dst-zone "trust"
  src-addr "Any"
  dst-addr "Any"
  service "Any"
exit
rule id 3
  action permit
  src-addr "Any"
  dst-addr "Any"
  service "Any"
  name "1"
exit
rule id 4
  action permit
  src-zone "trust"
  dst-zone "DMZ"
  src-addr "Any"
  dst-addr "Web"
  service "ICMP"
  service "HTTP"
  service "RDP"
exit
rule id 5
  action permit
  src-zone "untrust"
  dst-zone "DMZ"
  src-addr "Any"
  dst-addr "WAI"
  service "HTTP"
exit
l2-nonip-action drop
alg auto
no alg sip
tcp-mss all 1448
snmp-server host 20.10.28.100 version 2c community VcYqaeDeh6QdsqDBto
ecmp-route-select by-src-and-dst
strict-tunnel-check
statistics-set "predef_if_bw"
  target-data bandwidth id 0 record-history
  group-by interface directional
exit
statistics-set "predef_user_bw"
  target-data bandwidth id 1 record-history
  group-by user directional
exit
statistics-set "predef_app_bw"
  target-data bandwidth id 2 record-history
  group-by application
exit
statistics-set "predef_user_app_bw"
  target-data bandwidth id 3
  group-by user directional interface zone application
exit
statistics-set "predef_zone_if_app_bw"
  target-data bandwidth id 4
  group-by interface zone directional application
exit

End
DCFW-1800#                                                           

```