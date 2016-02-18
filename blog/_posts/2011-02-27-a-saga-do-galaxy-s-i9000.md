---
layout: post
title:  A saga do Galaxy S i9000
summary: O dia em que meu Galaxy S i9000 virou um tijolo.
date:   2011-02-27 03:16:35 -0300
categories: android
---

Recentemente adquiri um Galaxy S i9000, conhecido como a versão   internacional. Um aparelho notável, como tela de alta resolução, ótimo   processador e rodando o Android `2.1` Eclair.

Logo após a compra veio a boa notícia, a Samsung anunciou que forneceria atualizações para a versão `2.2` do sistema, conhecida como Android Froyo. Para mim e muitos outros, a maior vantagem da atualização seria permitir o uso do aplicativo [Skype][skype], visto que ele só funciona a partir da versão Froyo.

Aparelho atualizado e tudo funcionando bem. Mas não quer dizer que precise ficar assim, basta um pouquinho de ousadia... Pronto, resolvi tirar a versão oficial e testar algum *mod* personalizado e ver qual seria o resultado.

A princípio, percebi os seguintes obstáculos:

1. Essa versão do Galaxy só possui acesso ao modo de recuperação via *software*.
2. O modo de recuperação da versão `2.2` oficial (conhecido como *e3*) só permite a utilização de pacotes assinados pelo fabricante.

A solução do 1º obstáculo é simples, basta utilizar o comando `adb recovery mode` com o aparelho conectado via USB. Já o 2º obstáculo foi bem mais trabalhoso. Inicialmente tentei realizar um *downgrade* do modo de recuperação, passando-o para a versão *e2*, mas a operação falhou. Então resolvi ser mais agressivo e realizar o *downgrade* do Android para a versão `2.1`.

Devido a um problema na imagem do *firmware* utilizado, o aparelho entrou em *brick*. E assim acabei conhecendo a famosa *Black Screen of Death*:

![Black Screen of Death]({{ site.blog_assets_path }}/black-screen-of-death.png)

## O que seria *brick*?

Em poucas palavras, seu aparelho se torna inútil e muito semelhante a um tijolo... E o pior, essa versão do Galaxy também só entra em
modo de *download*, utilizado para atualizar o *firmware*, via *software*. Resumindo, não é possível atualizar seu aparelho, já que não há acesso ao sistema.

Foi então que encontrei este excelente vídeo e tudo foi resolvido.

<div class="embed-container"><iframe src="https://www.youtube-nocookie.com/embed/D0aoabVtPJ8?rel=0" frameborder="0" allowfullscreen></iframe></div>

Consegui colocar o Android `2.1` e aplicar o Insanity *mod* via [ROM Manager ClockworkMod][rom-manager]. O *mod* rodou bem, inclusive com algumas otimizações no gasto de bateria. Contudo, perde-se muitos aplicativos disponíveis especialmente para a versão oficial do Galaxy S. Até existem algumas formas de colocá-los de volta, mas não é como na versão oficial.

Então retornei ao Android `2.1`, via atualização de *firmware*, para fazer a atualização oficial utilizando o programa *Samsung Kies*. Tudo ocorreu bem, sendo que dessa vez eles já tinham disponibilizado a versão `2.2.1`. Mas qual seria a razão para se disponibilizar a versão `2.2.1` poucos dias depois?

Descobri da pior maneira. Já não era mais possível adquirir acesso `root` através do aplicativo [Z4Root][z4-root], que faz todo o procedimento pelo próprio aparelho. Por sorte, acabei encontrando o [SuperOneClick][super-one-click] e o mundo voltou a sorrir. Com ele é possível obter o acesso *root* na versão `2.2.1`, o único detalhe é a utilização de um sistema Windows para o mesmo.

Atualmente, estou satisfeito com o estado do aparelho, mas não descarto a possibilidade de no futuro voltar a testar algum outro *mod*, afinal o [CyanogenMod][cyanogen-mod] ainda não foi lançado oficialmente para o i9000. Medo de *brick*? Não, já sei como utilizar os resistores :smiley:

[skype]:           "http://www.skype.com"
[rom-manager]:     "https://market.android.com/details?id=com.koushikdutta.rommanager"
[z4-root]:         "http://forum.xda-developers.com/showthread.php?t=833953"
[super-one-click]: "http://forum.xda-developers.com/showthread.php?t=803682"
[cyanogen-mod]:    "http://www.cyanogenmod.com"
