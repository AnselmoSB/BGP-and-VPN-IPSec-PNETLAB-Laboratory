## Configuração das interfaces e do protocolo BGP no Cisco Router 1841

```bash
enable
configure terminal

! Interface WAN
interface Ethernet0/0
 description WAN-ISP
 ip address 198.51.100.2 255.255.255.252
 no shutdown
 exit

! Interface LAN
interface Ethernet0/1
 description LAN
 ip address 172.16.20.1 255.255.255.0
 no shutdown
 exit

! Configuração BGP (AS 65000)
router bgp 65000
 bgp log-neighbor-changes
 neighbor 198.51.100.1 remote-as 65100
 ! Anunciamos a rede pública conectada na nossa WAN para o ISP
 network 198.51.100.0 mask 255.255.255.252
 exit
exit
```

<br><br>


## Configuração do IPSEC no Cisco Router 1841

```bash
configure terminal

# ==========================================
# 1. CONFIGURAÇÃO DO ISAKMP (FASE 1)
# ==========================================
crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400
 exit

crypto isakmp key SenhaSegura123 address 203.0.113.2

# ==========================================
# 2. CONFIGURAÇÃO DO IPSEC (FASE 2)
# ==========================================
# Definindo o modo como transporte (essencial para casar com o Mikrotik)
crypto ipsec transform-set TS-GRE esp-aes 256 esp-sha256-hmac
 mode transport
 exit

crypto ipsec profile PROF-GRE
 set transform-set TS-GRE
 set pfs group14
 exit

# ==========================================
# 3. CRIAR A INTERFACE DE TÚNEL (GRE + IPsec)
# ==========================================
interface Tunnel0
 description TUNEL-PARA-MIKROTIK
 ip address 10.255.255.2 255.255.255.252
 tunnel source Ethernet0/3
 tunnel destination 203.0.113.2
 tunnel mode gre ip
 tunnel protection ipsec profile PROF-GRE
 no shutdown
 exit

# ==========================================
# 4. ROTEAMENTO DA LAN REMOTA
# ==========================================
ip route 192.168.10.0 255.255.255.0 10.255.255.1
end
write
```
