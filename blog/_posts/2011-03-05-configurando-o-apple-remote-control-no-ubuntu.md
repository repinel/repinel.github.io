---
layout: post
title:  Configurando o Apple Remote Control no Ubuntu
summary: Dicas de como configurar e tirar melhor proveito do Apple Remote Control em um MacBook com Ubuntu.
date:   2011-03-05 19:56:10 -0300
categories: linux mac
---

Mais um artigo da série [Instalando o Ubuntu Minimal no MacBook]({% post_url 2011-03-04-instalando-o-ubuntu-minimal-no-macbook %}). Agora apresentando os passos de configuração do controle remoto infravermelho do MacBook no Ubuntu.

Para o reconhecimento do infravermelho e ativação dos comandos que deverão ser executados para cada tecla do controle, utilizaremos o pacote [LIRC (Linux Infrared Remote Control)][lirc]. No Ubuntu, ele pode ser instalado, juntamente com os utilitários, a partir dos comandos como super usuário:

~~~ sh
aptitude -y install lirc lirc-x xautomation
mkdir /etc/lirc/lirc
~~~

Após a instalação, basta criar o arquivo de configuração para começar a utilizar o controle de remoto. O arquivo de configuração fica localizado em `/etc/lirc/lirc/lircrc` e contém o mapeamento dos comandos que serão executados para cada ação do controle. Meu arquivo pode ser baixado [aqui][my-lirc-config]. A seguir são exibidos dois blocos de configuração desse arquivo.

~~~ config
begin
  prog = irexec
  button = PLAY
  config = xte 'key XF86AudioPlay'
end

begin
  prog = irxevent
  button = PLAY
  config = Key F5 CurrentWindow
  repeat = 0
end
~~~

O primeiro bloco é ativado quando o `daemon irexec` está executando, já o segundo é ativado quando o `irxevent` está executando. Ambos representam ações que deve ser executadas quando botão PLAY for utilizado. Sendo o primeiro bloco responsável por simular a tecla *play* do teclado, e o segundo a tecla F5.

No meu caso, eu sempre ativo o `irexec` para funções multimídias e o `irxevent` para apresentações. Contudo, como ativar e desativar os *daemons* não era uma tarefa prática, resolvi criar o um GNOME Applet específico para trabalhar com o LIRC, o [ir-switcher][ir-switcher]. Para instalá-lo, basta executar os comandos como super usuário:

~~~ sh
wget -O ir_switcher.deb http://ir-switcher.googlecode.com/files/ir_switcher_0.1_all.deb
dpkg -i ir_switcher.deb
rm ir_switcher.deb
~~~

Depois é preciso adicioná-lo em um painel do seu ambiente, conforme a imagem.

![ir-switcher Panel]({{ site.blog_assets_path }}/ir-switcher-panel.png)

Clicando com o botão direito do mouse sobre o ícone é possível escolher qual *daemon* executar: `irexec`, `irxevent` ou os dois. Para iniciar a execução do *daemon* selecionado, clique com o botão esquerdo do mouse sobre o ícone e mesmo mudará, indicando a ativação.

Um artigo mais completo sobre as diferentes configurações do LIRC pode ser achado no [Viva o Linux][lirc-viva-o-linux]. Qualquer dúvida sobre a utilização do ir-switcher, não deixem de entrar em contato.

[lirc]:              http://www.lirc.org
[my-lirc-config]:    https://gist.github.com/repinel/fe58ca5216af90eaebf8
[ir-switcher]:       https://github.com/repinel/ir-switcher
[lirc-viva-o-linux]: http://www.vivaolinux.com.br/artigo/LIRC-Linux-Infrared-Remote-Control
