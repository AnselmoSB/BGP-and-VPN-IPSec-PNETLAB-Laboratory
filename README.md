## Construindo uma topologia de rede com implementação de eBGP e IPSec

O Objetivo desse laboratório é testar a comunicação via eBGP e IPSEC

<br>

### TOPOLOGIA
<img width="1050" height="586" alt="Topology_1" src="https://github.com/user-attachments/assets/682bb968-d4dc-4545-bc6d-aea580277d8a" />

<br>

### PRÉ-REQUISITOS
O ambiente que utilizo para rodar esse laboratório consiste dos seguintes itens:
1. VMWARE Workstation
2. Máquina Virtual com PNETLab instalado e rodando
3. Imagens dos Roteadores Cisco 1841 e Mikrotik RouterOS7

<br>

Os equipamentos podem ser configurados no ambiente do PNETLAB em qualquer ordem, mas a melhor ordem para verificar passo-a-passo o funcionamento das configurações é:
1. Router-ISP
2. Cisco-1841 (Configuração básica)
3. Mikrotik (Configuração básica)
4. Cisco-1841 (Configuração IPSEC)
5. Mikrotik (Configuração IPSEC)

<br>

### CONCLUSÃO
Após configurar todos os dispositivos o teste de ping entre as redes do Cisco-1841 e Mikrotik tem que ser bem sucedido
<img width="1201" height="964" alt="pnetlab-vpn-ipsec-test" src="https://github.com/user-attachments/assets/57798b24-e358-4850-87e1-ff0981f342ec" />
