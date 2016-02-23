---
layout: post
title:  Configurando a webcam iSight no Ubuntu
summary: Configurando a *webcam* do MacBook no Ubuntu.
date:   2011-03-05 19:03
categories: linux mac
---

Continuando o artigo [Instalando o Ubuntu Minimal no MacBook]({% post_url 2011-03-04-instalando-o-ubuntu-minimal-no-macbook %}), descreverei como configurar a *webcam* [iSight][isight] do MacBook no Ubuntu.

O driver utilizado pelo [Kernel][kernel] para se conectar a câmera é proprietário, mas pode ser obtido a partir do sistema MacOS instalado no próprio MacBook. Considerando, por exemplo, que o MacOS está instalado em `/dev/sda1`, podemos utilizar os seguintes comandos como super usuário:

~~~ sh
cd
mkdir /mnt/macosx
mount /dev/sda1 /mnt/macosx
cp /mnt/macosx/System/Library/Extensions/IOUSBFamily.kext/Contents/PlugIns/AppleUSBVideoSupport.kext/Contents/MacOS/AppleUSBVideoSupport .
umount /mnt/macosx
~~~

Dessa forma, teremos uma cópia do arquivo `AppleUSBVideoSupport` na *home* do super usuário. Agora precisaremos extrair o *firmware* do driver e instalá-lo.

~~~ sh
add-apt-repository ppa:mactel-support/ppa
aptitude update
aptitude upgrade
aptitude -y install isight-firmware-tools
cp AppleUSBVideoSupport /lib/firmware/
ift-extract --apple-driver /lib/firmware/AppleUSBVideoSupport
rm /lib/firmware/AppleUSBVideoSupport
~~~

Tendo executado esses comandos o *firmware* da webcam já deve estar disponível para ser carregar na próxima vez em que o sistema iniciar. Para testar a captura da imagem, uma boa dica é o programa [Cheese][cheese]. Ele pode ser instalado diretamente pelo próprio terminal.

~~~ sh
aptitude -y install cheese
~~~ 

Lembrando que ao atualizar a versão atual do Kernel, o *firmware* deve ser novamente instalado.

[isight]: http://en.wikipedia.org/wiki/ISight
[kernel]: http://www.kernel.org
[cheese]: http://projects.gnome.org/cheese
