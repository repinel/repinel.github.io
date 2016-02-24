---
layout:      post
title:       Acessando partições Ext4 no Mac OS X
description: Acessando a partição Ext4 do Ubuntu no Mac OS X com o `ext4fuse`.
date:        2011-03-05 20:40
categories:  linux mac
---

Há algum tempo estive procurando por uma solução eficiente para acessar os dados da partição Ext4 do Ubuntu no Mac OS X (veja [Instalando o Ubuntu Minimal no MacBook]({% post_url 2011-03-04-instalando-o-ubuntu-minimal-no-macbook %})). O objetivo era encontrar algo semelhante ao projeto [fuse-ext2][fuse-ext2], que além de facilitar a configuração do ambiente, permite montar automaticamente tanto HD internos e externos.

Acabei encontrando o projeto [ext4fuse][ext4fuse], criado e mantido por Gerard Lledó. O ext4fuse é normalmente disponibilizado como código fonte, diretamente do GitHub, e depende do MacFUSE devidamente compilado para ser compilado. Contudo, não consegui compilar as dependências utilizando a última versão do Xcode do Mac OS X Leopard :-(

~~~
$ ./core/macfuse_buildtool.sh -t smalldist
failed to determine Xcode version.
~~~

Não tendo como resolver a situação, entrei em contato com o Gerard e obtive o [binário do ext4fuse][ext4fuse-bin]. Com ele consegui habilitar corretamente a leitura de partições Ext4. Para configuração do ambiente, baixe o binário e execute os seguintes comandos no diretório onde ele se encontra:

~~~ sh
sudo cp ext4fuse-24810919 /usr/local/bin/ext4fuse
sudo ln -s /usr/local/bin/ext4fuse /sbin/mount_fuse-ext4
~~~

Dessa forma, querendo montar, por exemplo, a partição `/dev/disk0s2` em `/tmp/discoExt4`, pode-se utilizar os seguintes comandos:

~~~ sh
sudo mkdir /tmp/discoExt4
sudo mount -t fuse-ext4 /dev/disk0s2 /tmp/discoExt4
~~~

O mesmo pode ser automatizado criando-se um *script* com os comandos e executando-o ao iniciar o sistema.

[fuse-ext2]:    https://github.com/alperakcan/fuse-ext2
[ext4fuse]:     https://github.com/gerard/ext4fuse
[ext4fuse-bin]: https://github.com/downloads/gerard/ext4fuse/ext4fuse-24810919
