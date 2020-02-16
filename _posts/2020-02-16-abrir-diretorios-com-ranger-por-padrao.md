---
layout: post
title: "Abrir diretorios com o Ranger por padrão"
tags: ranger, tui, terminal, urxvt, arch, xdg
---

Ranger é meu Gerenciador de Arquivos favorito, especialmente combinado com i3wm que também é meu daily driver. É meu favorito porque é rápido, é inspirado com os keybinds do vim, e também é bastante configurável.

Mas o ponto aqui não é falar, nem convencer ninguém a usar o Ranger, é mais uma documentação de como estabelecer que o Ranger seja o padrão para abrir diretórios em todas as aplicações.

Por ele ser um app baseado em terminal (terminal user inteface) ele precisa rodar em algum emulador de terminais, no meu caso a minha escolha por padrão é o urxvt, ou rxvt-unicode. Aqui também não vem ao caso determinar as vantagens e desvantagens do rxvt, isso fica para uma próxima.

O problema é que mesmo se o ranger estiver definido no XDG como o app padrão, alguns outros apps, como por exemplo Firefox, qbitorrent, vão ignorar isso com o `.desktop` file padrão do ranger. Ex:

{% highlight bash %}
$ xdg-mime query filetype ~/
inode/directory
{% endhighlight %}

{% highlight bash %}
$ xdg-mime query default inode/directory 
ranger.desktop
{% endhighlight %}

Mesmo com essa situação, o qbittorrent ignoraria o ranger e mandaria abrir um diretorio no Firefox, por exemplo, especialmente se você não tiver outro Gerenciador de Arquivos instalado.

Para resolver o problema é trivial, primeiro se copia o arquivo `.desktop` do ranger para o local applications

{% highlight bash %}
$ cp /usr/share/applications/ranger.desktop ~/.local/share/applications
{% endhighlight %}

O arquivo deve estar algo próximo disso:

{% highlight ini %}
[Desktop Entry]
Type=Application
Name=ranger
Comment=Launches the ranger file manager
Icon=utilities-terminal
Terminal=true
Exec=ranger
Categories=ConsoleOnly;System;FileTools;FileManager
MimeType=inode/directory;
{% endhighlight %}

Vamos editar agora o ~/.local/share/applications/ranger.desktop:

{% highlight ini %}
Terminal=false
Exec=urxvt -e ranger
{% endhighlight %}

Ficando:

{% highlight ini %}
[Desktop Entry]
Type=Application
Name=ranger
Comment=Launches the ranger file manager
Icon=utilities-terminal
Terminal=false
Exec=urxvt -e ranger
Categories=ConsoleOnly;System;FileTools;FileManager
MimeType=inode/directory;
{% endhighlight %}

Importante notar para a linha  Exec=urxvt -e ranger, isso vale para mim que uso urxvt, caso você use outro emulador de terminal, você deve editar de acordo, e principalmente verificar na documentação se o parâmetro `-e` é o correto para execução do programa.
