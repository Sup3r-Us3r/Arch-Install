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

**Porquê Arch ?**

Uma das maiores vantagens da distribuição Arch Linux é a sua simplicidade na abordagem e atitude. O [Arch Linux Beginner's Guide](https://wiki.archlinux.org/index.php/Installation_guide_(Portugu%C3%AAs)) descreve esta atitude muito bem isso, ele lhe dá a capacidade de construir o seu sistema a partir do zero.


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

# PARTICIONAMENTO DE DISCO
Vamos usar o cfdisk para a criação das partições
```
# cfdisk /dev/sdX
```
(Substitua o X pela letra do seu disco rígido ex: 'sda' 'sdb') use: lsblk

<kbd>Particionamento de Disco **BIOS**</kbd>
<kbd>:large_orange_diamond: Particionamento de Disco **(UEFI)**</kbd>

# FORMATANDO AS PARTIÇÕES
Se o disco rígido estiver pronto e particionado de acordo com as suas necessidades, pode movê-lo formatando-o.

Formatar a partição sda1 (/root)
```
# mkfs.ext4 /dev/sda1
```

Ativar a partição SWAP
```
# mkswap /dev/sda2
# swapon /dev/sda2
```

:large_orange_diamond: Formate a partição sdaX (/boot) (UEFI) (Se for UEFI a partição /boot será a sda1)
```
# mkfs.ext4 /dev/sda1
# mkfs.fat -F32 -n BOOT /dev/sda1
```

# MONTAGEM DAS PARTIÇÕES
Antes de podermos baixar e instalar os pacotes base do Arch Linux precisamos montar nossas partições e mudar para o nosso diretório root. Afinal, este é onde vamos instalar o Arch Linux.

Montagem da partição root
```
# mount -t ext4 /dev/sda1 /mnt
```

:large_orange_diamond: Agora monte a partição: (/boot) (UEFI)
```
# mkdir -p /mnt/boot/efi && mount /dev/sdaX /mnt/boot/efi
```
Verifique as partições com este comando
```
# lsblk /dev/sdX
```
(Substitua o X pela letra do seu disco rígido ex: 'sda' 'sdb')

# ESCOLHER O ESPELHO DE DOWNLOAD
Escolher a lista de espelhos mais próxima
```
# pacman -Sy
# pacman -S reflector
# reflector --verbose -l 5 --sort rate --save /etc/pacman.d/mirrorlist
```

# INSTALAR OS PACOTES BASE DO ARCH LINUX
```
# pacstrap -i /mnt base base-devel
```

# CONFIGURAR O FSTAB
Para configurar fstab (tabela de sistemas de arquivos) execute:
```
# genfstab -U -p /mnt >> /mnt/etc/fstab
```
Você deve sempre verificar se a entrada fstab está correta ou não, que será capaz de inicializar em seu sistema. Para verificar a entrada fstab, execute:
```
# cat /mnt/etc/fstab
```
Se tudo estiver OK você deve ver o root montado.


# NOVO SISTEMA
Agora é hora de mudar para o diretório root recém-instalado para configurá-lo.
```
# arch-chroot /mnt
# loadkeys br-abnt2
```

# CONFIGURAR KEYMAP
A variável KEYMAP é especificada no arquivo /etc/vconsole.conf . Ele define qual layout de teclado, será usado nos consoles virtuais. Execute este comando:
```
# echo -e 'KEYMAP="br-abnt2.map.gz"' > /etc/vconsole.conf
```

# CONFIGURAÇÕES DE IDIOMA E FUSO HORÁRIO
Para configurar o idioma do sistema, execute o seguinte comando:
```
# sed -i  '/pt_BR/,+1 s/^#//' /etc/locale.gen
# locale-gen
# echo LANG=pt_BR.UTF-8 > /etc/locale.conf
# export LANG=pt_BR.UTF-8
```
Para ver todos os fusos horários disponíveis da América:
```
# ls /usr/share/zoneinfo/America
```
Agora você pode configurar a sua zona:
```
# ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
```
Vamos agora configurar o relógio do hardware, apenas no caso de termos uma data errada:
```
# hwclock --systohc --utc
```
Para conferir se a hora está certa:
```
# date
```

# CONFIGURAR REPOSITÓRIO
Com este comando habilitamos o repositório multilib:
```
# sed -i '/multilib\]/,+1 s/^#//' /etc/pacman.conf
# pacman -Sy
```

# DEFINIR HOSTNAME
```
# echo arch > /etc/hostname
```

# CONFIGURANDO A CONEXÃO
Ethernet:
```
# systemctl enable dhcpcd
```
Wifi:
```
# pacman -S wpa_supplicant wpa_actiond dialog iw networkmanager
# systemctl enable NetworkManager
```

# CRIAR USUÁRIO
* useradd -m -g [initial_group] -G [additional_groups] -s [login_shell] [username]
```
# useradd -m -g users -G wheel,storage,power -s /bin/bash ghost
```
Em seguida, forneça a senha para este novo usuário executando:
```
# passwd ghost
```
Não se esqueça de definir também a senha para o usuário **root**:
```
# passwd
```
Permitir que os usuários no grupo wheel, sejam capazes de executar tarefas administrativas com o sudo:
```
sed -i '/%wheel ALL=(ALL) ALL/s/^#//' /etc/sudoers
```
