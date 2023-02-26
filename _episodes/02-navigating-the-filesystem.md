---
タイトル: "ファイルシステムの操作"
講義: 20
演習: 10
課題:
- "シェルの中でどのように移動しますか？"
目標:
- "シェルコマンドを使ったディレクトリとファイルの操作"
- "コマンドを使ったデータの検索と操作"
キーポイント:
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
それは基本的なコマンドに、さまざまな「**フラグ**」（オプション、パラメーター、あるいはよく引数（ひきすう）とも呼ばれる）を指定することで得られます。
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

Notice that the command didn't output anything. This means that it was carried
out successfully. Let's check by using `pwd`:

~~~
$ pwd
~~~
{: .bash}
~~~
/Users/riley/Desktop
~~~
{: .output}

If something had gone wrong, however, the command would have told you. Let's
test that by trying to move into a non-existent directory:

何か間違った操作をしても教えてくれます。

cd Desktop Windows や　

~~~
$ cd "things to learn about the shell"
~~~
{: .bash}
~~~
bash: cd: things to learn about the shell: No such file or directory
~~~
{: .output}

Notice that we surrounded the name by quotation marks. The *arguments* given
to any shell command are separated by spaces, so a way to let them know that
we mean 'one single thing called "things to learn about the shell"', not
'six different things', is to use (single or double) quotation marks.

We've now seen how we can go 'down' through our directory structure
(as in into more nested directories). If we want to go back, we can type `cd ..`.
This moves us 'up' one directory, putting us back where we started.
**If we ever get completely lost, the command `cd` without any arguments will bring
us right back to the home directory, the place where we started.**

今いるところがわからなくなったらcdで自分のホームディレクトリに戻ります。
スペースで区切ります。6つの違ったものではないという場合はダブルクォートあるいはシングルクォートで囲みます。

ディレクトリ構造を下っていっている。ディレクトリの奥にはいっていく。ディレクトを戻りたかったら、ピリオド2つを打ちます。
こうするとディレクトリを一つ上に移動します。

> ## Previous Directoryまえに行ったディレクトリ
> To switch back and forth between two directories use `cd -`.　cd マイナスというコマンドを使います。
{: .callout}

> ## Try exploring
>
> Move around the computer, get used to moving in and out of directories,
> see how different file types appear in the Unix shell. Be sure to use the `pwd` and
> `cd` commands, and the different flags for the `ls` command you learned so far.

コンピュータの中をみてみましょう。どんな種類のファイルがUnix Shellを使ってみてみましょう。
>
> If you run Windows,
> also try typing `explorer .` to open Explorer for the current directory
> (the single dot means "current directory"). If you're on a Mac,
> try `open .` and for Linux try `xdg-open .` to open their graphical file manager.
もしWindowsを使っている人は

>
{: .challenge}


Being able to navigate the file system is very important for using the Unix shell effectively.
As we become more comfortable, we can get very quickly to the directory that we want.

すごく速くできちゃう・・・。

> ## Getting help
>
> Use the `man` command to invoke the manual page (documentation) for a shell command.manコマンドでマニュアルを表示できます。
> For example, `man ls` displays all the arguments available to you - which saves
> you remembering them all! Try this for each command you've learned so far.調べてみましょう。
> Use the <kbd>spacebar</kbd> to navigate the manual pages. Use <kbd>q</kbd> at any time to quit.読み進めるにはスペースキーで、qで終了します。

> ***Note*: this command is for Mac and Linux users only**. It does not work directly for Windows users.
> If you use Windows, you can search for the shell command on [http://man.he.net/](http://man.he.net/),
> and view the associated manual page. In some systems the command name followed by `--help` will work, e.g. `ls --help`.　いくつかのコマンドでは --helpででてくる
>
> Also, the manual lists commands individually, e.g., although `-h` can only be used together with the `-l` option,  
> you'll find it listed as `-h` in the manual, not as `-lh`.
>
> >## Answer　答え
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


> ## Find out about advanced `ls` commands
>
> Find out, using the manual page, how to list the files in a
> directory ordered by their filesize. Try it out in different directories. Can you combine it
> with the `-l` *argument* you learned before?
>
> Afterwards,
> find out how you can order a list of files based on their last modification date.
> Try ordering files in different directories.
>
> > ## Answer
> >
> > To order files in a directory by their filesize, in combination with the `-l` argument:
> >
> > ~~~
> > ls -lS 答えはS
> > ~~~
> > {: .bash}
> >
> > Note that the `S` is **case-sensitive!**
> >
> > To order files in a directory by their last modification date, in combination with the `-l` argument:
> >
> > ~~~
> > ls -lt　答えはt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}
