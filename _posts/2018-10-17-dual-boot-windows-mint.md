---
title: Dual-boot com Windows 10 e Linux Mint 18.3
date: 2018-10-17 11:11:11 -0300
lastmod: 2020-06-10 04:20:00 -0300
author: Valdinei Ferreira
categories: [Tutorial, Tech]
tags: [linux,dual-boot]
---

Resumindo a história: todo mundo me ensinava usando Linux, daí precisei usar o SO, mas não queria largar os jogos, então o dual-boot era a solução.
Sem enrolação, vamos ao processo. Só preste atenção à data de minha publicação, não me responsabilizo por nada.

## Passo-a-passo
Seguirei o processo do meu ponto de vista.
1. Gravei o OS em um disco (pode ser flash drive, obviamente);
2. Separei um espaço não-alocado no programa nativo do Windows para gerenciar no instalador do OS. Não esquecendo de desfragmentar (pode ser com o nativo do Windows também), senão podia demorar muito mais para particionar;
3. Desativar o Secure Boot ou, como fiz, mudei o Secure Boot de "Windows UEFI Mode" para "Other OS" (essa opção provavelmente só apareça na UEFI da Asus);
4. Separei o sistema Linux em 3 partições, porém há muitas divergências de como prosseguir com isso, mas acredito que seja obrigatório apenas a `/` (raiz). Uns dizem que a partição swap é apenas recomendado, outros dizem que pode ocorrer falhas se não existir: 
	* `/boot`: Coloquei 300MB, primária e ext2, e eu selecionei ele para o carregador de inicialização;
	* `swap`: Coloquei 2GB, se não me engano coloquei como lógico (ainda não sei a diferença);
	* `/`: Onde será instalado o sistema.
5. Prossegui com a instalação normalmente e reiniciei;
6. No meu caso, o Windows não apareceu no GRUB, então segui o que o site [2] recomendou. Funcionou!



## Dicas e Tutoriais

Completo + Informações detalhadas sobre partições (tipo, sysfile)  
[1] https://www.vivaolinux.com.br/dica/Dual-boot-UEFI-Ubuntu-e-Windows-8

Alterar tempo de escolha e sistema preferencial (para ser inicializado quando acabar o tempo)  
[3] https://www.vivaolinux.com.br/dica/GRUB-2-Ubuntu-1404-Editando-nome-sistema-default-e-tempo-de-boot#comentario238436  
**Obs. 1**: seguir as orientações do comentário 4, de Gleno Machado;  
**Obs. 2**: reiniciar o Apache com "sudo service apache2 restart";



## Resolução de Problemas

Caso o Windows não apareça no GRUB  
[2] http://www.meurock.com/?p=6293

Caso apareça o "initramfs" ou "BusyBox" quando escolher Linux no GRUB  
```sudo fdisk -l | grep Linux | grep -Ev 'swap'```  
[4] https://askubuntu.com/questions/137655/boot-drops-to-a-initramfs-prompts-busybox  
[5] http://www.proposedsolution.com/solutions/ubuntu-booting-to-initramfs-prompt/  
[6] https://askubuntu.com/questions/88384/how-can-i-repair-grub-how-to-get-ubuntu-back-after-installing-windows  
[7] https://askubuntu.com/questions/207663/cannot-update-grub-with-parameters-on-live-usb#333011