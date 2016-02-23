---
layout: post
title:  Uma breve análise sobre redes Wi-Fi
summary: Algumas dicas para tirar melhor proveito de redes Wi-Fi.
date:   2011-02-27 15:32:59 -0300
categories: android network
---

O aumento do número de residências com redes Wi-Fi tem se tornado cada vez mais nítido. Espero não ser o único a ficar surpreso ao verificar a lista de redes próximas a mim. Mas quais os malefícios dessa sobrecarga? O que fazer para melhorar o sinal da sua rede?

Considerando que o espectro destinado ao conjunto de redes Wi-Fi é limitado, por normas ou *hardware*, quanto maior o número redes maior será a densidade do meio e a interferência entre elas. Muitos programas permitem a listagem das redes Wi-Fi disponíveis, mas alguns outros são capazes de detalhar ainda mais. O programa [Wi-Fi Analyzer][wi-fi-analyzer], disponível para plataformas Android, é um deles. A partir dele podemos obter o seguinte gráfico.

![Wifi Analyzer Graph]({{ site.blog_assets_path }}/wifi-analyzer-graph.png)

No eixo X, temos a disposição dos [canais de redes IEEE 802.11][wlan-channels]. Lembrando que no Brasil é apenas permitido canais entre 1 e 11. Já no eixo Y, temos uma estimativa da força do sinal das redes. Redes que compartilham mesmo canal estão sujeitas a maior colisão de pacotes e perda da eficiência. Portanto, uma boa iniciativa é verificar qual o canal mais adequado, aquele com menor compartilhamento e interseção entre as parábolas. O programa Wi-Fi Analyzer se torna útil novamente, possibilitando a geração uma tela mais elaborada para a indicação de canais que poderiam ser escolhidos.

![Wifi Analyzer Channels]({{ site.blog_assets_path }}/wifi-analyzer-channels.png)

A análise dos canais disponíveis é apenas um dos pontos passiveis de melhoramento. Outros truques, como o posicionamento do roteador, podem ser verificado no [artigo da PC WORLD][pc-world-article].

[wi-fi-analyzer]:   https://market.android.com/details?id=com.farproc.wifi.analyzer
[wlan-channels]:    http://en.wikipedia.org/wiki/List_of_WLAN_channels
[pc-world-article]: http://pcworld.uol.com.br/dicas/2008/08/27/cinco-truques-para-melhorar-o-sinal-de-sua-rede-sem-fio
