Roteador ISP

A configuração do Router ISP é bem simples com as configurações das interfaces e BGP configurado apenas para o lado do Cisco Router 1841

```bash
enable
configure terminal
hostname Internet-Core

! Interface ligada ao Mikrotik (Link Dedicado)
interface Ethernet0/0
 description LINK-MIKROTIK
 ip address 203.0.113.1 255.255.255.252
 no shutdown
 exit

! Interface ligada ao Cisco (Link BGP)
interface Ethernet0/1
 description LINK-CISCO
 ip address 198.51.100.1 255.255.255.252
 no shutdown
 exit

! Sessão BGP com o Cisco (AS 65100 simulando o Provedor)
router bgp 65100
 bgp log-neighbor-changes
 neighbor 198.51.100.2 remote-as 65000
 ! Envia uma rota padrão (0.0.0.0/0) para o Cisco Edge
 neighbor 198.51.100.2 default-originate
 exit```
