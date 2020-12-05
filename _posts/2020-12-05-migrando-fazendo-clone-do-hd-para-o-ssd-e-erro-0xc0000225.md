---
title: Migrando (fazendo clone) do HD para o SSD (e erro 0xc0000225)
author: Valdinei Ferreira
date: 2020-12-05T19:13:54.124Z
lastmod: 2020-12-05T19:13:54.146Z
categories:
  - Tutorial
tags:
  - tutorial
  - windows
  - hd
  - ssd
  - clone
  - "0xc0000225"
  - diskpart
---
## Fazendo o clone

Para fazer o clone do HDD para o SSD, eu utilizei o software **Macrium Reflect**. Você inclusive consegue baixar a versão gratuita sem inserir seu e-mail (tanto para baixar, quanto para instalar).

Após instalar este software, você talvez consiga fazer sozinho, mas vou tentar descrever os passos que fiz:

1. Antes de tudo, remova arquivos indesejados/desnecessários e cache, para não pesar na migração e ela ser mais rápida. Ou mova certos arquivos para uma partição segura (que não está envolvida na clonagem).
2. Selecione o disco onde está a instalação do Windows;
3. Ao selecionar o disco desejado, aparecerá a opção "Clone this disk...". Clique nela;
4. Selecione suas partições a serem clonadas e o disco de destino (com todas partições excluídas, se já estiverem sido criadas);
5. Clique em "Copy selected partitions"
6. Se não aparecer nenhuma mensagem de erro ou aviso, pule para o passo #. Se der erro, é porque o espaço total das partições (não o espaço utilizado) é maior do que o que está disponível no disco de destino. Neste caso, você vai ter que copiar as partições de uma em uma e redimensionar com o Reflect para caber todas as partições.
7. Agora é só finalizar. Dependendo da quantidade de arquivos existentes, vai demorar algumas horas. Eu tinha mais de 100GB e demorou mais de 4 horas, pois não removi alguns arquivos e pastas enormes.
8. Após esperar o tempo de clonagem, você já pode ver pelo Gerenciamento de Disco e pelo Macrium Reflect a nova estrutura das partições.

## Resolvendo o erro 0xc0000225

Se não quiser texto, pule para o subtítulo.

Fui inventar de deletar as partições do Windows do HD (isso depois de reiniciar o PC e bootar pelo SSD, pra ter certeza que deu certo), aparentemente tinha dado certo. Continuei a mexer no PC. Depois de um tempo, decidi reiniciar e deu uma tela azul avisando que o Windows precisava de reparo, com o código do título acima. Foi aí que procurei soluções, como a que descrevi abaixo.

### Prompt de Comando

Eu tinha uma ISO relativamente antiga do Windows 10, mas usei assim mesmo, na pressa de resolver o problema. Ao iniciar o PC pelo drive bootável, iniciei o prompt de comando e fiz as seguintes etapas:

1. Entrei no DiskPart, digitando **`diskpart`** no terminal;
2. Selecione o drive onde está a instalação do Windows com **`select disk #`**, sendo # o número do disco, que pode ser verificado com o comando **`list disk`**;
3. Digite **`detail disk`** para ver as partições do disco;
4. Selecione o volume/partição onde está o disco de inicialização (o meu tem cerca de 260MB, com Rótulo "System" e é FAT32) com **`select volume #`**, com # sendo o número informado na execução do passo anterior;
5. Digite **`assign letter=#`**, sendo # uma letra que desejar, mas que não tenha sido utilizada. Eu utilizei a letra J.
6. Após definir uma letra para a partição, saia do DiskPart e digite o comando **`bcdboot #:\windows /l pt-br /s $:`**, sendo # a letra do disco onde está o Windows (a pasta Windows) e $ a letra que escolheu anteriormente para a partição de inicialização do sistema.
7. Após isto, deve aparecer uma mensagem de sucesso. Daí é só reiniciar o PC e pronto!

## Links Úteis

Utilizei os sites abaixo para a clonagem. Talvez te ajude como complemento.

\[1] [COMO TRANSFERIR SEU WINDOWS DE HD PRA SSD COMO SE NADA TIVESSE MUDADO - YouTube](https://www.youtube.com/watch?v=14hgO8VUitM)

\[2] [Erro boot/load W10 após migrar HD para SSD - Microsoft Community](https://answers.microsoft.com/pt-br/windows/forum/windows_10-other_settings/erro-bootload-w10-ap%C3%B3s-migrar-hd-para-ssd/c7e67d22-00b7-4128-938d-e69fdbefb995)

\[3] [DiskPart to assign drive letters via Command Prompt - Robert Karp (google.com)](https://sites.google.com/site/robertjkarp/home/windows/diskpart-to-assign-drive-letters-via-command-prompt)