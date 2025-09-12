<!--
 " Badges ------------------ {{{
 -->
 <!-- Estes badges só funcionarão quando o repositório do github for público -->
[![GNS3 Version](https://img.shields.io/badge/GNS3-v3.2.6-blue?logo=https://upload.wikimedia.org/wikipedia/commons/thumb/3/3d/GNS3_logo.png/128px-GNS3_logo.png)
![GNS3 Version](https://img.shields.io/npm/v/gns?style=flat&logo=https://raw.githubusercontent.com/GNS3/gns3-gui/master/resources/icons/gns3.png&label=GNS) ![Qemu Version](https://img.shields.io/npm/v/qemu?style=flat&logo=qemu&logoColor=white&label=Qemu)](https://www.qemu.org) ![Repo size](https://img.shields.io/github/repo-size/ernanikern70/Qemu-GNS3-Tutorial?label=Repo%20size&style=flat-round) ![GitHub branch status](https://img.shields.io/github/checks-status/ernanikern70/Qemu-GNS3-Tutorial/main) ![GitHub stars](https://img.shields.io/github/stars/ernanikern70/Qemu-GNS3-Tutorial?label=Stars&style=flat-round&color=yellow) ![Last commit](https://img.shields.io/github/last-commit/ernanikern70/Qemu-GNS3-Tutorial?label=Last%20commit&style=flat-round&color=green) ![Open Issues](https://img.shields.io/github/issues/ernanikern70/Qemu-GNS3-Tutorial?style=flat-round&color=red) ![Open PRs](https://img.shields.io/github/issues-pr/ernanikern70/Qemu-GNS3-Tutorial?style=flat-round&color=orange) ![Latest Release](https://img.shields.io/github/v/release/ernanikern70/Qemu-GNS3-Tutorial?style=flat-round&color=brightgreen) <!-- ![Topics](https://img.shields.io/github/topics/ernanikern70/Qemu-GNS3-Tutorial?style=flat-round&color=purple&cacheSeconds=30) -->

---
<!--
" }}}
-->
<!--
" Sumário ---------- {{{
-->
### Sumário

- [Introdução](#introdução)
- [Ambiente](#ambiente)

---
<!--
" }}}
-->
<!--
" Introdução --------------- {{{
-->
# GNS3 Tutorial

Tutorial pessoal de GNS3 com Qemu, pode conter dados equivocados ou desatualizados.

---
<!--
" }}}
-->
<!--
" Ambiente --------------------- {{{
-->
## Ambiente

### Downloads:  

- [GNS3](https://www.gns3.com/software/download)  

O Qemu deve ser instalado automaticamente, caso negativo: 

- [Qemu](https://www.qemu.org/)  

Para testes de rede neste projeto, usei VMs do Ubuntu Server e Bodhi Linux (mais leves) no formato Qemu (.qcow2).  
A VM Bodhi Linux foi baixada diretamente de [Osboxes](https://osboxes.org), no formato .vdi e convertida para .qcow2 com o comando _qemu-img convert_.  
A VM do Ubuntu Server foi instalada do zero, pois a imagem do Osboxes era muito 'crua' e estava trazendo dificuldades.

A instância NAT do GNS3 é a que provê o serviço DHCP às VMs, através da interface __virbr0__ que deve estar ativa no host (rede 192.168.122.0/24 - virbr0 = 192.168.122.1).

### Configurações do servidor: 

- Ubuntu Server 24.04

    - Interface WAN (ens3) 192.168.122.10/24: conectada à instância NAT do GNS3 (Internet)

    - Interface LAN (ens4) 10.255.255.1/28: conectada à rede interna
    
As configurações acima já permitem ao servidor acessar a Internet.  

Para permitir a navegação ao cliente: 

```
sudo sysctl -w net.ipv4.ip_forward=1_
echo 'net.ipv4.ip_forward=1' | sudo tee /etc/sysctl.d/99-ipforward.conf >/dev/null
sudo sysctl --system

sudo iptables -t nat -A POSTROUTING -s 10.255.255.0/28 -o ensX -j MASQUERADE
sudo iptables -A FORWARD -i ensY -o ensX -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A FORWARD -i ensX -o ensY -m state --state ESTABLISHED,RELATED -j ACCEPT
            
sudo apt install iptables-persistent -y
sudo netfilter-persistent save
```

Configurações de rede via Netplan: 

```
$$ cat 01-net-cfg.yaml 
network:
  version: 2
  renderer: networkd
  ethernets:
    ens3: # WAN
      dhcp4: false
      addresses:
        - 192.168.122.10/24
      routes:
        - to: default
          via: 192.168.122.1
     nameservers:
       addresses:
         - 8.8.8.8
         - 127.0.0.53
    ens4:   # LAN
      dhcp4: false
      addresses: 
        - 10.255.255.1/28
```

### Configurações das estações clientes: 

- Bodhi Linux 7.0.0

    - Credenciais padrão da imagem Osboxes: 'osboxes/osboxes.org'

    - Interface de rede (LAN): 10.255.255.5/28

Configurações de rede via Netplan: 

```
$$ cat 00-installer-config.yaml 

# This is the network config written by 'subiquity'
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    ens3:
      dhcp4: false
      addresses:
        - 10.255.255.5/28
      routes: 
        - to: default
          via: 10.255.255.1
      nameservers:
        addresses:
          - 8.8.8.8
```

### Configurações no host

    - Interface _virbr0_ 192.168.122.1: configurada pelo GNS3/Qemu

Se desejar conexão direta aos clientes do GNS3: 

```
sudo ip route add 10.255.255.0/28 via 192.168.122.10
```

Para tornar permanente, editar _/etc/netplan/_ do host, ou em _/etc/systemd/network/_ (dependendo do renderizador).


---
<!--
" }}}
-->
