## Configuração básica de conexão do Mikrotik

```bash
/system identity set name=Mikrotik-Edge

# Configuração das Interfaces LAN e WAN
/ip address
add address=192.168.10.1/24 interface=ether2 comment="LAN"
add address=203.0.113.2/30 interface=ether4 comment="WAN-Dedicado"

# Rota Default apontando para o ISP
/ip route
add distance=1 dst-address=0.0.0.0/0 gateway=203.0.113.1

# NAT Básico de Internet
/ip firewall nat
add action=masquerade chain=srcnat out-interface=ether1 comment="NAT-Internet"
```

<br>
<br>

## Configuração do túnel, IPSEC e roteamento no Mikrotik:

```bash
# ==========================================
# 1. CRIAR A INTERFACE DE TÚNEL GRE
# ==========================================
/interface gre add name=gre-to-cisco remote-address=198.51.100.2 local-address=203.0.113.2 keepalive=10s,3

# ==========================================
# 2. ENDEREÇAMENTO IP DO TÚNEL
# ==========================================
/ip address add address=10.255.255.1/30 interface=gre-to-cisco

# ==========================================
# 3. CONFIGURAÇÃO DO IPSEC (FASE 1 e FASE 2)
# ==========================================
# Fase 1: Perfil de Criptografia
/ip ipsec profile add name=profile-cisco dh-group=modp2048 enc-algorithm=aes-256 hash-algorithm=sha256 lifetime=1d

# Fase 1: Definição do Peer (Vizinho)
/ip ipsec peer add name=peer-cisco address=198.51.100.2/32 profile=profile-cisco exchange-mode=main

# Fase 1: Chave Pré-Compartilhada (Pre-Shared Key)
/ip ipsec identity add peer=peer-cisco secret="SenhaSegura123"

# Fase 2: Proposta de Criptografia (Transform-set)
/ip ipsec proposal add name=prop-cisco auth-algorithms=sha256 enc-algorithms=aes-256-cbc pfs-group=modp2048 lifetime=30m

# Fase 2: Política para interceptar e criptografar APENAS o tráfego GRE (Protocolo 47)
/ip ipsec policy add src-address=203.0.113.2/32 dst-address=198.51.100.2/32 protocol=gre action=encrypt proposal=prop-cisco tunnel=no peer=peer-cisco

# ==========================================
# 4. BYPASS DO NAT (EXEMPÇÃO)
# ==========================================
# Garante que o tráfego LAN-to-LAN não sofra Masquerade. Deve ficar no topo (index 0).
/ip firewall nat add chain=srcnat action=accept src-address=192.168.10.0/24 dst-address=172.16.20.0/24 place-before=0

# ==========================================
# 5. ROTEAMENTO DA LAN REMOTA
# ==========================================
/ip route add dst-address=172.16.20.0/24 gateway=10.255.255.2
```
