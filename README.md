---
título: Arch Install
data: 18-08-2017
página inicial: https://github.com/Sup3r-Us3r
download: https://www.archlinux.org/download/
demo: https://youtube.com/...
autor: Magno Tutor
---

# INSTALAÇÃO ARCH LINUX

Este guia destina-se a ajudar alguém a instalar a distribuição Arch Linux  em seu Computador. O guia pressupõe que você tenha alguma familiaridade com o sistema linux e esteja confortável, trabalhando a partir da linha de comando, mas não exige que você seja um especialista. Aprendemos muito fazendo e se você quiser saber mais sobre como o linux opera, o Arch Linux é uma excelente opção por muitas razões.

**Porquê Arch?**

Uma das maiores vantagens da distribuição Arch Linux é a sua simplicidade na abordagem e atitude. O [Arch Linux Beginner's Guide](https://wiki.archlinux.org/index.php/Installation_guide_(Portugu%C3%AAs)) descreve esta atitude muito bem isso, ele lhe dá a capacidade de construir o seu sistema a partir do zero


**Os princípios de design por trás do Arch são destinados a mantê-lo simples**

>«Simples», neste contexto, significa «sem adições, modificações ou complicações desnecessárias». Em resumo; Uma abordagem elegante e minimalista.


**Alguns pensamentos a ter em mente ao considerar a simplicidade:**

>"Simples" é definido de um ponto de vista técnico, não um ponto de vista de usabilidade. É melhor ser tecnicamente elegante com uma curva de aprendizado mais alta, do que ser fácil de usar e tecnicamente [inferior]. "- Aaron Griffin

>"A parte extraordinária de [meu método] reside em sua simplicidade ... A altura do cultivo sempre corre para a simplicidade". - Bruce Lee

------

* Faça o download do Arch Linux: <kbd>[AQUI](https://www.archlinux.org/download/)</kbd>

* Para criar um USB bootable no Linux/Windows use o: <kbd>[Etcher](https://etcher.io/)</kbd> ou <kbd>[Rufus](https://rufus.akeo.ie)</kbd> - <kbd>[Win32DiskImager](https://sourceforge.net/projects/win32diskimager/)</kbd>

* Para criar um USB bootable usando o comando (dd) no Linux:

```
# dd bs=4M if=/lugar_onde_esta_sua_iso of=/dev/sdX status=progress && sync
```

(Substitua o X pela letra do seu dispositivo ex: 'sdc' 'sdd') use: lsblk

------

**`Observação: Caso você necessite instalar via UEFI os comandos estão com o simbolo`** :large_orange_diamond:

`No particionamento UEFI, faça como segue a foto de particionamento UEFI, em seguida monte as partições de acordo com o particionamento feito.`

------

:large_orange_diamond: **Verifique o modo de inicialização: (UEFI)**
```
# efivar -l
```
Se este comando listar as **variáveis EFI**, isso significa que você iniciou a operação com sucesso no modo **EFI**. Caso contrário, reinicie no **menu de boot** novamente e selecione o item correto lá, e não o item **legacy-mode**.

Se o diretório não existir, o sistema pode ser inicializado no modo **BIOS** ou **CSM**.

# CONEXÃO COM A INTERNET
`Ethernet`
```
# systemctl start dhcpcd
# ping -c3 google.com
```
`Wifi`
```
# wifi-menu
# ping -c3 google.com
```

