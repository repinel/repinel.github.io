---
layout: post
title:  Instalando o Ubuntu Minimal no MacBook
summary: Guia simplificado de como instalar o Ubuntu em um MacBook.
date:   2011-03-04 16:51
categories: linux mac
---

As máquinas da Apple não são apenas conhecidas pelo design clean e arrojado, elas também possuem ótima arquitetura computacional. Seus portáteis, como os MacBooks, são reconhecidos por serem mais leves que a maioria da categoria, o que justifica o grande sucesso.

Mesmo aqueles que não são usuários exclusivos do sistema MacOS acabam optando por esses equipamentos. Mas como deixar seu sistema mais próximo do desempenho do MacOs em MacBooks? Como sugestão, volto ao artigo [Tirando o melhor do Ubuntu]({% post_url 2011-02-27-tirando-o-melhor-do-ubuntu %}), que aborda as características do sistema [Ubuntu Minimal][ubuntu-minimal-cd].

Algum tempo atrás nem todas as funcionalidades dos MacBooks podiam ser exploradas, como utilizar multi-touch no trackpad e controlar o brilho da tela. Mas hoje, além dessas funcionalides, já temos o aperfeiçoamento da duração bateria em sistemas Linux, devido a novas características do [Kernel][kernel].

O passo inicial para a instalação de um segundo sistema operacional em qualquer máquina é o particionamento, ou reparticionamento. No MacOS, isso pode ser feito através do programa [BootCamp][boot-camp], utilizado na instalação do Windows. O BootCamp pode ser encontrado na pasta Utilities, e como queremos instalar o Ubuntu, iremos utilizá-lo somente até o reparticionamento. Recomendo deixar pelo menos 5GB para o Ubuntu, conforme a imagem.

![Bootcamp]({{ site.blog_assets_path }}/boot-camp.png)

Após ajeitar o espaço em disco, precisamos permitir o boot do novo sistema. Como a tabela de partições do MacBook não exatamente como a de PCs, não podemos utilizar diretamente o GRUB ou o LILO, assim instalaremos o [rEFit][refit]. Basta baixar o pacote [DMG][refit-dmg] da última versão, montá-lo e abrir o `rEFit.mpkg`.

![rEFIt Installer]({{ site.blog_assets_path }}/rEFIt-installer.png)

Para que a instalação tenha efeito, desligue o MacBook, **não reinicie**. Ao ligá-lo novamente, deverá aparecer uma tela semelhante a seguinte.

![Boot MacOS]({{ site.blog_assets_path }}/boot-macos.png)

Com isso, já podemos continuar a instalação do Ubuntu. Baixe e grave a imagem do [Ubuntu Minimal][ubuntu-minimal-cd] em um CD. **É importante lembrar que durante todo o processo de instalação seu computador deve estar conectado a internet**, pois os pacotes do sistema serão baixados sob demanda.

Para realizar o boot do CD, pressione e segure a `tecla C` ao ligar o MacBook. **Para que o boot ocorra corretamente, sempre desligue a máquina antes de tentar entrar pelo CD, não apenas reinicie**. Dentre as opções, escolha Install.

![Ubuntu Minimal Installer]({{ site.blog_assets_path }}/ubuntu-minimal-installer.png)

Durante o processo de instalação, certifique-se de escolhar o teclado correto. Utilizei o *USA Macintosh* durante a instação, mas atualmente utilizo o *USA International (with dead keys)* devido a acentuação.

Outro ponto importante é a escolha de onde instalar o GRUB, o *boot loader*. Ele deve ser instalado na mesma partição do Ubuntu. Se, por exemplo, o Ubuntu foi instalado em `/dev/sda2`, então o GRUB também deve ser instalado em `/dev/sda2`.

Seguindo esses passos, Ubuntu deve ser reconhecido pelo rEFit e essa tela aparecerá ao ligar o MacBook.

![Boot MacOS and Linux]({{ site.blog_assets_path }}/boot-macos-linux.png)

Ao iniciar o Ubuntu, ele consistirá apenas do terminal, sem gerenciador de janelas. Nesse ponto, volto a sugerir o [*script*][ubuntu-minimal-script] do grupo [Minimal Desktop for Ubuntu][ubuntu-minimal-desktop]. As configurações especiais para o MacBook serão abordadas em posts separados, devido ao aumento da complexidade. Contudo, deixo meu [*scrip*][my-ubuntu-minimal-script] disponível, visto que ele já possui algumas indicações interessantes.

[ubuntu-minimal-cd]:        https://help.ubuntu.com/community/Installation/MinimalCD
[kernel]:                   http://www.kernel.org
[boot-camp]:                http://www.apple.com/support/bootcamp
[refit]:                    http://refit.sourceforge.net
[refit-dmg]:                http://sourceforge.net/projects/refit/files/rEFIt
[ubuntu-minimal-desktop]:   http://minimal-desktop.blogspot.com
[ubuntu-minimal-script]:    https://github.com/AntonioPT/minimal-desktop-for-ubuntu/blob/e799996f02aba1947329cbd57ce343b3848a4431/script.sh
[my-ubuntu-minimal-script]: https://gist.github.com/repinel/8f15e5acb4fe8a08ecdd
