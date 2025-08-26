# Qemu-GNS3-Tutorial

Tutorial pessoal de GNS3 com Qemu, pode conter dados equivocados ou desatualizados.

<!--
 " Badges ------------------ {{{
 -->

 <!-- Estes badges só funcionarão quando o repositório do github for público -->
 [![Qemu Version](https://img.shields.io/npm/v/qemu?style=flat&logo=qemu&logoColor=white&label=Qemu)](https://www.qemu.org) ![GNS3 Version](https://img.shields.io/npm/v/gns?style=flat&logo=gns3&label=GNS) ![Repo size](https://img.shields.io/github/repo-size/ernanikern70/Qemu-GNS3-Tutorial?label=Repo%20size&style=flat-round) ![GitHub branch status](https://img.shields.io/github/checks-status/ernanikern70/Qemu-GNS3-Tutorial/main) ![GitHub stars](https://img.shields.io/github/stars/ernanikern70/Qemu-GNS3-Tutorial?label=Stars&style=flat-round&color=yellow) ![Last commit](https://img.shields.io/github/last-commit/ernanikern70/Qemu-GNS3-Tutorial?label=Last%20commit&style=flat-round&color=green) ![Open Issues](https://img.shields.io/github/issues/ernanikern70/Qemu-GNS3-Tutorial?style=flat-round&color=red) ![Open PRs](https://img.shields.io/github/issues-pr/ernanikern70/Qemu-GNS3-Tutorial?style=flat-round&color=orange) ![Latest Release](https://img.shields.io/github/v/release/ernanikern70/Qemu-GNS3-Tutorial?style=flat-round&color=brightgreen) <!-- ![Topics](https://img.shields.io/github/topics/ernanikern70/Qemu-GNS3-Tutorial?style=flat-round&color=purple&cacheSeconds=30) -->

---
<!--
" }}}
-->
<!--
" Ambiente --------------------- {{{
-->
## Ambiente

##### Downloads:  

- [GNS3](https://www.gns3.com/software/download)  

O Qemu deve ser instalado automaticamente, caso negativo: 

- [Qemu](https://www.qemu.org/)  

Para testes de rede neste projeto, usei imagens do Ubuntu Server 24.04 baixadas do site [OsBoxes.org](https://osboxes.org), que são bastante 'cruas' - tem apenas o básico do S.O. instalado, sem ping, vi, nano, edit, etc. Para conectar o servidor à Internet, editei o arquivo '/etc/netplan/01-net-cfg.yaml' conforme abaixo e conectei a VM a uma instância 'Nat' do GNS3.    


---
<!--
" }}}
-->
