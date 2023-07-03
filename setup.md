---
layout: page
title: "セットアップ"
---

このライブラリーカーペントリーのレッスンに参加するには、UNIXのシェル環境が必要です。

具体的には、LinuxとmacOSに標準搭載されているBash（[Bourne Again Shell](https://en.wikipedia.org/wiki/Bash_(Unix_shell))）を使用します。macOS Catalinaユーザーはzsh（Zシェル）がデフォルトバージョンとなります。

WindowsユーザーであってもBashを学ぶことで、個人のマシンで強力なツール群を利用できるようになり、さらに、ほとんど全てのサーバーやスーパーコンピュータで使われている標準的なリモートインターフェースに慣れることができます。

>## 端末のセットアップ
>
>Bashは、ほとんどのLinuxディストリビューションとmacOSでデフォルトのシェルとして使用されています。
>Windowsユーザーは、UNIX環境を提供するために Git Bash をインストールする必要があります。
>
>- **Linux:** 　デフォルトのシェルは通常Bashですが、マシンの設定が異なる場合は、ターミナルを開いて`bash`と入力することで実行することができます。何もインストールする必要はありません。Bashシェルを起動するには、アプリケーションでTerminal（端末）を探します。
>
>- **macOS:** 　Bashは、Catalina以前のmacOSの全てのバージョンでデフォルトのシェルです。何もインストールする必要はありません。ターミナルを `/Applications/Utilities` から開くか、スポットライト検索でBashシェルを起動します。
>
>- **Windows:** 　Windowsでは、通常CMD（コマンドプロンプト）または PowerShell がデフォルトのシェル環境として利用可能です。これらはWindowsシステム特有の構文とアプリケーションのセットを使用し、より広く使用されているUNIXユーティリティとは互換性がありません。しかし、BashシェルをWindowsにインストールすることで、UNIXに似た環境を提供することができます。このレッスンでは、[Git for Windows](https://gitforwindows.org/)パッケージの一部である Git Bash を使用することをお勧めします：
>    - 最新のGit for Windows [installer](https://gitforwindows.org/)をダウンロードしてください。（訳注：Git SCMをダウンロードしてください）
>    - `.exe`をダブルクリックすると、デフォルトの設定でインストーラー（例：`Git-2.13.3-64-bit.exe`）を実行します。
>    - インストールしたら、スタートメニューからGit Bashを選択してシェルを開きます（Gitフォルダの中にあります）。
>
>また、Windows上でBashコマンドを実行するための、より高度なソリューションも用意されています。Windows 10 では、Bash シェルのコマンドラインツールが提供されており、[Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10)を有効にしている場合に使用できます。
> 
>さらに、すでにUnixシェルを持っているリモートコンピュータやサーバーで、WindowsマシンからBashコマンドを実行することができます。これは通常、Secure Shell (SSH)クライアントを介して行うことができます。 Windowsコンピュータで無料で利用できるこのようなクライアントの1つに、[PuTTY](https://www.putty.org/)があります。
>
>もし問題が発生した場合、カーペントリーズは[Configuration Problems and Solutions wiki page](https://github.com/carpentries/workshop-template/wiki/Configuration-Problems-and-Solutions)を管理しており、助けになるかもしれません。
{: .prereq}

>## Data Files
>このレッスンに参加するためには、いくつかのファイルをダウンロードする必要があります：
>1. [shell-lesson.zip]({{ page.root }}/data/shell-lesson.zip) をダウンロードし、デスクトップに移動します。
>2. ファイルを解凍／展開します（このステップでサポートが必要な場合は、インストラクターに尋ねてください）。デスクトップに`shell-lesson`という新しいフォルダができるはずです。
>3. ターミナルを開き、`ls`と入力し、<kbd>enter</kbd>キーを押してください。
>   
>~~~~
>$ ls
>~~~~
>{: .bash}
>
> カレントディレクトリにあるフォルダーをご覧ください。（訳注：デスクトップ上の`shell-lesson`が見つからない場合は、`explorer.exe .`（起動ファイル名、半角空白、半角ピリオド）と入力し、<kbd>enter</kbd>キーを押してください。エクスプローラーが起動するので、その中に`shell-lesson`のフォルダを移動させます。`shell-lesson`がデスクトップに解凍されていない場合は、エクスプローラーを開いてshell-lessonを検索してみてください）　4. それからタイプします：
>~~~~
>$ pwd
>~~~~
>{: .bash}
>
> このコマンドは、ファイルシステムのどこにいるのかを表示します。このレッスンでは、`ls`、`pwd`というコマンドと、`shell-lesson`フォルダのデータの扱い方について詳しく説明します。
{: .prereq}

[template]: {{ site.workshop_repo }}
