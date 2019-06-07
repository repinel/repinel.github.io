---
layout:      post
title:       Gerando plugins NPAPI no Windows
description: Guia simplificado de como gerar plugins NPAPI, para *browsers* como Chrome e Firefox, no Windows.
date:        2011-03-16 01:10
categories:  web windows
---

Muitas das extensões para Firefox e Chromium, que são limitadas pelo suporte da API, acabam recorrendo a plugins [NPAPI][npapi] para solucionar seus problemas. Plugins NPAPI são programas capazes de serem acessador via JavaScript, e podem ser escritos em linguagens como C++, que facilita em muito certos trabalhos.

Contudo, a maioria dos plugins possui um núcleo comum, que consiste de sua integração com o navegador. Assim, por que não escrever apenas as funcionalidades que efetivamente caracterizam o plugin? Isso pode ser obtido com o uso de *frameworks*, como [Nixysa][nixysa] e [Firebreath][firebreath].

Apesar do *framework* Nixysa ser mais simples, a pouca documentação torna o trabalho um pouco mais complicado, principalmente para criação de plugins NPAPI em ambientes Windows, as conhecidas DLLs. Assim, descreverei como gerar o plugin a partir do projeto exemplo `Complex`.

Primeiramente, precisaremos instalar algumas dependências:

1. Um cliente do Subversion, recomendo o [Win32Svn][win32-svn].
2. Python `2.7.1`.
3. [PyWin32-216][py-win32] para Python `2.7`.
4. Se seu sistema for o Windows XP, instale o Service Pack 3.
5. Microsoft Visual C++ 2005 Express Edition.
6. Microsoft Platform SDK.

Após a instalação das dependências, seguindo as opções padrão, adicione alguns diretório ao `PATH` do ambiente. Verifique se os caminhos utilizados são compatíveis com seu Windows.

* `C:\Python27`
* `C:\Program Files\Subversion\bin`

Abra o Prompt de Comando, pressione `Win+E` e digite `cmd`. Digite os seguintes comandos no terminal:

~~~
cd \
mkdir Sources
cd Soruces
git clone https://github.com/google/nixysa.git nixysa-read-only
~~~

Descrição dos comandos por linha:

1. Abre a raiz do seu sistema.
2. Cria o diretório `Sources`, onde o projeto será baixado. Você pode alterar o nome do diretório, mas ele não deve conter espaços ou qualquer caracter especial.
3. Entra no diretório recém criado.
4. Baixa a última versão do repositório do Nixysa, atualmente a `versão 75`.

Utilizando o comando `dir`, é possível verificar que o `nixysa-read-only` foi criado. Depois abra o Microsoft Visual Solution (`complex.sln`) do projeto exemplo.

~~~
cd nixysa-read-only\examples\complex
start complex.sln
~~~

[![Visual C++]({{ site.blog_assets_path }}/visual-cpp.png){:height="154" :width="300"}]({{ site.blog_assets_path }}/visual-cpp.png)

Configure o VC++ para trabalhar com o Microsoft Platform SDK. Entre em `Tools -> Options -> Project and Solutions -> VC++ Directories` e adicione os seguintes diretórios:

* Executable files: `C:\Program Files\Microsoft Platform SDK\Bin`
* Include files: `C:\Program Files\Microsoft Platform SDK\Include`
* Library files: `C:\Program Files\Microsoft Platform SDK\Lib`

[![Visual C++ Options]({{ site.blog_assets_path }}/visual-cpp-options.png){:height="173" :width="300"}]({{ site.blog_assets_path }}/visual-cpp-options.png)

Agore adicione algumas definições para o preprocessador. Entre em `Project -> Configuration -> C/C++ -> Preprocessor` e adicione `_X86_;X86` a `Preprocessor Definitions`.

[![Complex Property]({{ site.blog_assets_path }}/complex-property.png){:height="300" :width="204"}]({{ site.blog_assets_path }}/complex-property.png)

Abra o arquivo `complex.rc` e substitua a linha `#include "afxres.h"` por `#include "windows.h"`. Agora bastar pressionar `F7` para realizar o `build` do projeto. A DLL `npcomplex.dll` poderá ser encontrada no diretório `nixysa-read-only\examples\complex\DEBUG`. O plugin também será automaticamente copiado para o diretório `%USERPROFILE%\Application Data\Mozilla\Plugins`, em que `%USERPROFILE%` representa o diretório do usuário.

Para testar o plugin, basta abrir o arquivo `nixysa-read-only\examples\complex\test.html` no Firefox. Lembrando que para testar em navegadores Chromium, a linha `<object type='application/complex' id='plugin' width='0' height='0'> </object>` deve ser substituída por `<embed type="application/complex" id="plugin" width='0' height='0'/>`.

Recomendo também verificar os [vídeos do Firebreath][firebreath-videos], a documentação é bem mais completa que o Nixysa.

[npapi]:             https://en.wikipedia.org/wiki/NPAPI
[nixysa]:            https://code.google.com/archive/p/nixysa/
[firebreath]:        https://code.google.com/archive/p/firebreath/
[win32-svn]:         https://sourceforge.net/projects/win32svn/
[py-win32]:          https://sourceforge.net/projects/pywin32/
[firebreath-videos]: https://github.com/firebreath/FireBreath

