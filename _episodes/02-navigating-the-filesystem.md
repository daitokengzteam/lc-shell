---
title: "ファイルシステムの操作"
teaching: 20
exercises: 10
questions:
- "シェルの中でどのように移動しますか？"
objectives:
- "シェルコマンドを使ったディレクトリとファイルの操作"
- "コマンドを使ったデータの検索と操作"
keypoints:
- "ディレクトリ構造のどこにいるのかを知ることはシェルで作業するときの鍵である"
---
## シェルの操作

シェルの操作の基本から始めましょう。

シェルを開いてみましょう。ドルマークの後にカーソルが点滅している黒い画面が開きます。
これがコマンドラインで、`$`（ドルマーク）は**コマンドプロンプト**になります。これは、入力する準備ができていることを示します。
見た目はシェルのセットアップ方法によって違うこともありますが、たいてい`$`で終わっています。

シェルを操作するとき、通常あなたはどこかのフォルダ（ディレクトリ）にいます。まずどこのフォルダにいるのか、`pwd`というコマンドを使って探しましょう。
`pwd`は"print working directory"の略で、コマンドの結果は標準出力である画面に表示されます。

さっそく`pwd`と入力してEnterキーを押してみましょう。（`$`は入力せずに、その後に続くものだけを入力することに注意してください。`$`は、コマンドプロンプトに入力することを示しています）


~~~
$ pwd
~~~
{: .bash}
~~~
/Users/riley
~~~
{: .output}

出力されるのは、ホームディレクトリへのパスです。このディレクトリの中身を見て確認してみましょう。`ls`コマンドを使います。これは"list"の略で、ディレクトリ内のコンテンツがすべて表示されます。

~~~
$ ls
~~~
{: .bash}
~~~
Applications Documents    Library      Music        Public
Desktop      Downloads    Movies       Pictures
~~~
{: .output}

ファイルやディレクトリのリストだけでなく、もっと情報が欲しいときもあります。
その場合、基本的なコマンドにさまざまな「**フラグ**」（オプション、パラメーター、あるいはよく引数（ひきすう）とも呼ばれる）を指定することで得られます。
引数は、どのような出力や操作が必要かをコンピュータに伝えることで、コマンドの動作を変更します。

`ls -l`と入力してEnterキーを押すと、コンピュータはFinder (Mac) や Explorer (Windows) にあるような情報を含むファイルのリストを返します。ファイルのサイズ（バイト数）や、作成日、最終更新日、ファイル名です。

~~~
$ ls -l
~~~
{: .bash}
~~~
total 0
drwx------+  6 riley  staff   204 Jul 16 11:50 Desktop
drwx------+  3 riley  staff   102 Jul 16 11:30 Documents
drwx------+  3 riley  staff   102 Jul 16 11:30 Downloads
drwx------@ 46 riley  staff  1564 Jul 16 11:38 Library
drwx------+  3 riley  staff   102 Jul 16 11:30 Movies
drwx------+  3 riley  staff   102 Jul 16 11:30 Music
drwx------+  3 riley  staff   102 Jul 16 11:30 Pictures
drwxr-xr-x+  5 riley  staff   170 Jul 16 11:30 Public
~~~
{: .output}

日常的には、キロバイト、メガバイト、ギガバイトのような単位をよく使います。
幸いなことに、`-h`フラグを`-l`と一緒に使用すると、バイト、キロバイト、メガバイト、ギガバイト、テラバイト、ペタバイトのような単位を付けて、サイズを 2 の累乗で表し、桁数を 3 以下にして表示します。（訳注: たとえばバイト数1564は、1.5Kと表示する）

ここでは`ls -h`を単独では使いません。2つのフラグを組み合わせたいときは、一緒に実行させることができます。つまり、`ls -lh`と入力してEnter キーを押すと、人間が読める形式の出力が得られます。(注意: ここでの順序は重要ではありません)

~~~
$ ls -lh
~~~
{: .bash}
~~~
total 0
drwx------+  6 riley  staff   204B Jul 16 11:50 Desktop
drwx------+  3 riley  staff   102B Jul 16 11:30 Documents
drwx------+  3 riley  staff   102B Jul 16 11:30 Downloads
drwx------@ 46 riley  staff   1.5K Jul 16 11:38 Library
drwx------+  3 riley  staff   102B Jul 16 11:30 Movies
drwx------+  3 riley  staff   102B Jul 16 11:30 Music
drwx------+  3 riley  staff   102B Jul 16 11:30 Pictures
drwxr-xr-x+  5 riley  staff   170B Jul 16 11:30 Public
~~~
{: .output}

私たちはホームディレクトリにかなり時間を費やしました。どこかへ移動しましょう。
`cd`またはディレクトリの変更コマンドを使っておこないます。（注意：WindowsとMacではデフォルトでファイル名やディレクトリ名の大文字・小文字は区別しませんが、Linuxでは関係があります。

~~~
$ cd Desktop
~~~
{: .bash}

このコマンドは何も出力しないことに注意してください。これはコマンドが正常に実行されたことを意味します。`pwd`を使って確認しましょう。

~~~
$ pwd
~~~
{: .bash}
~~~
/Users/riley/Desktop
~~~
{: .output}

もし何か間違っていたら、コマンドが教えてくれるはずです。存在しないディレクトリに移動することを試してみましょう。

~~~
$ cd "things to learn about the shell"
~~~
{: .bash}
~~~
bash: cd: things to learn about the shell: No such file or directory（そのようなファイルやディレクトリはありません）
~~~
{: .output}

名前を引用符で囲んでいることに注意してください。任意のシェルコマンドに与えられる「引数」はスペースで区切られているため、「6つの異なるもの」ではなく、「シェルについて学ぶこと」というひとつのものだと知らせるためにシングル、もしくはダブルコーテーションを使います。

ここまでディレクトリ構造を「下に」移動する方法を見てきました（つまり、ディレクトリの中のディレクトリに移動しています）。**もし戻りたい場合は`cd ..`と入力します。するとひとつ上のディレクトリに移動し、スタート地点に戻ります。**

今いるところがわからなくなったら、引数なしの`cd`コマンドで自分のホームディレクトリに戻ります。

> ## 前に行ったディレクトリ
> 2つのディレクトリを行き来するには、`cd -`を使います。
> 
{: .callout}

> ## 探索してみる
>
> コンピューターの中を動き回り、ディレクトリの中や外への移動に慣れ、
> UNIXシェルでさまざまなファイルタイプがどのように表示されるのかを見てみます。
> `pwd`と`cd`コマンド、これまでに学んだ`ls`コマンドの色々なフラグを必ず使用してください。
>
> もしWindowsを使っている人は、`explorer .`と入力すると、現在のディレクトリが
> エクスプローラーで開きます（最後のピリオドは「現在のディレクトリ」を意味します）。
> Macの場合は、`open .`、Linuxでは`xdg-open .`と入力すると、
> グラフィカルなファイルマネージャーが開きます。
>
{: .challenge}

ファイルシステムを操作できることは、UNIXシェルを効果的に使うために非常に重要です。慣れてくると、目的のディレクトリまで素早くたどり着くことができます。

> ## ヘルプの表示
> 
> `man`コマンドを使用して、シェルコマンドのマニュアルページ（ドキュメント）を呼び出します。
> 例えば、`man ls`とすると使用可能なすべての引数を表示します。これですべてを覚えておく必要がなくなります！
> 
> これまでに学んだコマンドを試してみましょう。
> マニュアルのページを移動するにはスペースキーを使います。終了するときは通常`q`を使います。

> ***注意：このコマンドは、MacとLinuxユーザー専用です。*** Windowsユーザーは直接使えません。
> Windowsを使用している場合は、[http://man.he.net/](http://man.he.net/)（訳注: manの日本語訳 [https://linuxjm.osdn.jp/](https://linuxjm.osdn.jp/) ）
> でそのシェルコマンドを検索し、関連するマニュアルページを見ることができます。一部のシステムでは、コマンドのあとに`ls --help`のように、`--help`をつけます。
>
> また、マニュアルにはコマンドが個別にリストされています。
> 例えば、`-h`は`-l`オプションと一緒にしか使用できませんが、マニュアルには`-lh`ではなく、`-h`として並んでいます。（訳注：日本語ヘルプ[JMプロジェクト](https://linuxjm.osdn.jp/)）
> 
>
> >## 回答
> >~~~
> >$ man ls
> >~~~
> >{: .bash}
> >~~~
> >LS(1)                     BSD General Commands Manual                    LS(1)
> >
> >NAME
> >     ls -- list directory contents
> >
> >SYNOPSIS
> >     ls [-ABCFGHLOPRSTUW@abcdefghiklmnopqrstuwx1] [file ...]
> >
> >DESCRIPTION
> >     For each operand that names a file of a type other than directory, ls
> >     displays its name as well as any requested, associated information.  For
> >     each operand that names a file of type directory, ls displays the names
> >     of files contained within that directory, as well as any requested, asso-
> >     ciated information.
> >
> >     If no operands are given, the contents of the current directory are dis-
> >     played.  If more than one operand is given, non-directory operands are
> >     displayed first; directory and non-directory operands are sorted sepa-
> >     rately and in lexicographical order.
> >
> >     The following options are available:
> >
> >     -@      Display extended attribute keys and sizes in long (-l) output.
> >
> >     -1      (The numeric digit ``one''.)  Force output to be one entry per
> >             line.  This is the default when output is not to a terminal.
> >
> >     -A      List all entries except for . and ...  Always set for the super-
> >             user.
> >
> >...several more pages...
> >
> >BUGS
> >     To maintain backward compatibility, the relationships between the many
> >     options are quite complex.
> >
> >BSD                              May 19, 2002                              BSD
> >
> >~~~
> >{: .output}
> {: .solution}
{: .challenge}


> ## `ls`コマンドの詳細を見る
>
> マニュアルページを使い、ディレクトリ内のファイルをファイルサイズ順にリストする方法を見つけてください。
> 他のディレクトリでも試してください。前に学んだ引数`-l`と組み合わせることができますか。
> 
> その後、最終更新日によりファイルのリストを並び替える方法を探してください。
> 別のディレクトリにあるファイルも並べ替えてみてください。
> 
>
> > ## 回答
> >
> > 引数`-l`と組み合わせて、ディレクトリの中のファイルサイズ順にファイルを並び替えます。:
> >
> > ~~~
> > ls -lS
> > ~~~
> > {: .bash}
> >
> > `S`は**大文字・小文字を区別する**ことに注意してください。
> >
> > 引数`-l`と組み合わせて、ディレクトリ内のファイルを最終更新順に並べます。:
> >
> > ~~~
> > ls -lt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}
