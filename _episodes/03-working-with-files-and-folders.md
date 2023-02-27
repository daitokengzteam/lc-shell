---
title: "ファイルやディレクトリを操作する " 
teaching: 20
exercises: 10
questions:
- "ファイルやディレクトリをコピー、移動、削除するにはどうすればいいのですか？"
- "ファイルを読むにはどうすればいいのですか？"
objectives:
- "コマンドラインからファイルとディレクトリを操作します。"
- "タブ補完を使い入力を確定します。"
- "コマンドを使い、ファイルとファイルの一部を出力し、表示します。"
- "コマンドを使い、ファイルの移動、名前の変更、コピー、削除をおこないます。"
keypoints:
- "シェルは、複数のファイルのコピー、移動、結合で使うことができます。"
---

## ファイルとフォルダで作業を進めます。
ディレクトリを操作するだけでなく、コマンドラインでファイルを操作することができます。
ファイルを読むこと、開くこと、実行すること、編集することができます。
実際、シェルの中でできることに制限はありません。
しかし、経験豊富なシェルユーザーでさえも、書式付きのテキストの編集、文書 (Word や OpenOffice) の編集、Web 閲覧、画像の編集など、多くの作業をグラフィカルユーザーインターフェース（GUI）に切り替えています。
しかし、もし何百枚もの画像、たとえばスキャンした本のページを同じように切り抜きたい場合は、シェルコマンドを使えば、切り抜き作業を自動化できます。

始める前に、`ls` を使い現在操作しているディレクトリ地を確認します。
定期的に `ls` を使い、操作対象のファイルの一覧を表示することは、これから自分がなにをするのかを確認するために有効です。

~~~
$ ls
~~~
{: .bash}
~~~
Applications Documents    Library      Music        Public
Desktop      Downloads    Movies       Pictures
~~~
{: .output}

ファイルと対話するためのいくつかの基本的な方法を試します。
最初に、デスクトップにある `shell-lesson` ディレクトリに移動してみましょう。

~~~
$ cd
$ cd Desktop/shell-lesson
$ pwd
~~~
{: .bash}
~~~
/Users/riley/Desktop/shell-lesson
{: .output}
~~~

ここでは、新しいディレクトリを作成し、そこに移動します。

~~~
$ mkdir firstdir
$ cd firstdir
~~~
{: .bash}

ここで、`mkdir`コマンド（`ディレクトリを作る`という意味）を使い、'firstdir'という名前のディレクトリを作りました。
それから、`cd`コマンドを使い、そのディレクトリの中に移動しました。

でも、ちょっと待ってください！
これを早くするトリックがあります。1つ上のディレクトリに移動しましょう。

~~~
$ cd ..
~~~
{: .bash}

`cd firstdir` とタイプする代わりに、`cd f` とタイプして Tab キーを押してみましょう。
シェルが `cd firstdir/` の行を補完していることに気づきます。

> ## タブの自動補完
> シェル内で任意のタイミングでタブを押すと、カレントディレクトリ内のファイルとサブディレクトリに基づき、行の自動補完を試みるようにプロンプトが表示されます。
> 2つまたはそれ以上のファイルで同じ文字がある場合、自動補完機能は最初の相違点までしか入力しません。
> その後に文字を追加し、再度Tabキーを使用して補完を試すことができます。
> 今日一日この方法を使い、動作を確認することをお勧めします（時間と労力が大幅に節約されます！）。
{: .callout}

### ファイルを読む

`firstdir` にいる場合は、`cd ..` を使い、 `shell-lesson`の ディレクトリに戻ります。

ここには、プロジェクトグーテンベルグ (https://www.gutenberg.org/) からダウンロードした2冊のパブリックドメインの図書のコピーと、後で紹介する他のファイルがあります。

~~~
$ ls -lh
~~~
{: .bash}
~~~
total 33M
-rw-rw-r-- 1 riley staff 383K Feb 22 2017  201403160_01_text.json
-rw-r--r-- 1 riley staff 3.6M Jan 31 2017  2014-01-31_JA-africa.tsv
-rw-r--r-- 1 riley staff 7.4M Jan 31 2017  2014-01-31_JA-america.tsv
-rw-rw-r-- 1 riley staff 125M Jun 10 2015  2014-01_JA.tsv
-rw-r--r-- 1 riley staff 1.4M Jan 31 2017  2014-02-02_JA-britain.tsv
-rw-r--r-- 1 riley staff 582K Feb  2 2017  33504-0.txt
-rw-r--r-- 1 riley staff 598K Jan 31 2017  829-0.txt
-rw-rw-r-- 1 riley staff  18K Feb 22 2017  diary.html
drwxr-xr-x 1 riley staff  64B Feb 22 2017  firstdir
~~~
{: .output}

ファイルの`829-0.txt`と`33504-0.txt`は、Project Gutenbergにある書籍#829と#33504の本文ファイルです。
どちらの本か忘れてしまったので、最初のファイルのテキストを読むために `cat` コマンドを試してみます。

~~~
$ cat 829-0.txt
~~~
{: .bash}

ターミナル画面が開き、本の内容全体が一度に流れ（ターミナルに表示されます）、新しいプロンプトが表示された後、このプロンプトの上に本の最後の数行が表示されます。

ファイルの最初や最後のところをざっと見て、そのファイルの内容を把握したいことはよくあります。
そのためにUnix シェルは `head` と `tail` というコマンドを用意しています。

~~~
$ head 829-0.txt
~~~
{: .bash}
~~~
The Project Gutenberg eBook, Gulliver's Travels, by Jonathan Swift


This eBook is for the use of anyone anywhere at no cost and with
almost no restrictions whatsoever.  You may copy it, give it away or
re-use it under the terms of the Project Gutenberg License included
with this eBook or online at www.gutenberg.org
~~~
{: .output}

これは、最初の10行を表示するもので、`tail 829-0.txt`は、最後の10行を提示します。

~~~
$ tail 829-0.txt
~~~
{: .bash}
Most people start at our Web site which has the main PG search facility:

    http://www.gutenberg.org

This Web site includes information about Project Gutenberg-tm,
including how to make donations to the Project Gutenberg Literary
Archive Foundation, how to help produce our new eBooks, and how to
subscribe to our email newsletter to hear about new eBooks.
~~~
{: .output}

10行では足りない (あるいは多すぎる) 場合は、 `man head` (あるいは Windows では `head --help`) をチェックしてください。
表示する行数を指定するオプションがあるかどうかを確認します。
(表示する行数を特定するオプションはあります: `head -n 20` は20行を表示します)。

ファイルを操作する別の方法は、一度に画面ずつ表示することです。
最初の画面を見るには `less 829-0.txt` と入力して、’スペースキー’で次の画面が見られます。 
`q`で終了（コマンドプロンプトに戻る）です。

~~~
$ less 829-0.txt
~~~
{: .bash}

他の多くのシェルコマンドと同様に、コマンド の`cat`、`head`、`tail`、`less` は任意の数の引数をつけることができます。
（これらは任意の数のファイルを処理できます）ここでは、複数のファイルの最初の行を一度に取得する方法について見てみます。
入力の手間を抑えるため、最初にとても便利なトリックを紹介します。

> ## コマンドの再利用
> 空白のコマンドプロンプトにて、上矢印キーを押すと、カーソルの前に前回入力したコマンドが表示されることがわかります。
> 上矢印キーを押し続けると、前のコマンドを順次表示させることができます。
> 下矢印キーを押すと、最新のコマンドに戻ります。これも重要な省力化機能で、これから頻繁に使います。
{: .callout}

上矢印を押し、`head 829-0.txt` コマンドを表示させます。
次にスペースを追加し、`33504-0.txt`（Tabキーを思い出してください、３と入力後にTabを入力すると`33504-0.txt`になります。）を入力します

~~~
$ head 829-0.txt 33504-0.txt
~~~
{: .bash}
~~~
==> 829-0.txt <==
The Project Gutenberg eBook, Gulliver's Travels, by Jonathan Swift


This eBook is for the use of anyone anywhere at no cost and with
almost no restrictions whatsoever.  You may copy it, give it away or
re-use it under the terms of the Project Gutenberg License included
with this eBook or online at www.gutenberg.org




==> 33504-0.txt <==
The Project Gutenberg EBook of Opticks, by Isaac Newton

This eBook is for the use of anyone anywhere at no cost and with
almost no restrictions whatsoever.  You may copy it, give it away or
re-use it under the terms of the Project Gutenberg License included
with this eBook or online at www.gutenberg.org


Title: Opticks
       or, a Treatise of the Reflections, Refractions, Inflections,
~~~
{: .output}

ここまではいいのですが、図書がたくさんある場合、すべてのファイル名を入力するのは面倒です。
幸いなことに、シェルはワイルドカードをサポートしています!
`?` (ちょうど1文字に一致) と `*` (0または0個以上の文字に一致) は、おそらく図書館の検索システムでもお馴染みです。
ワイルドカードの `*` を使えば、上記の `head` コマンドをよりコンパクトに記述できます。

~~~
$ head *.txt
~~~
{: .bash}

> ## ワイルドカードについて
> ワイルドカードはシェルの特徴で、 あらゆるコマンドで機能します。
> シェルはコマンドを実行する前に、ワイルドカードをファイルやディレクトリのリストに展開します。
> コマンドはそのワイルドカードを参照することはありません。
> 例外として、ワイルドカードの表現がどのファイルにもマッチしない場合、Bash  はその式をそのままコマンドのパラメータとして渡します。
> 例えば、ls *.pdf` と入力すると、エラーメッセージに*.pdfというファイルはありませんと表示されます。　
{: .callout}

<!-- Timing: Spent 75 minutes to get here -->

### ファイルの移動、コピー、削除

ファイル名をもっとわかりやすいものに変更したい場合があります。
この場合、 `mv` または move コマンドを使って、新しい名前に **move** することができ、最初の引数に古い名前、2番目の引数に新しい名前を指定します。

~~~
$ mv 829-0.txt gulliver.txt
~~~
{: .bash}

これは、「ファイル名の変更」機能に相当します。
その後で、`ls`コマンドを実行すると、`gulliver.txt`という名前になっていることが確認できます。

~~~
$ ls
~~~
{: .bash}
~~~
2014-01-31_JA-africa.tsv   2014-02-02_JA-britain.tsv  gulliver.txt
2014-01-31_JA-america.tsv  33504-0.txt
2014-01_JA.tsv
~~~
{: .output}

> ## ファイルのコピー
>
> ファイルを*移動*する代わりに、ファイルを*copy*（コピーを作成）したいとき、例えば、ファイルを変更する前にバックアップを作成する場合などです。
> mv` コマンドと同様に、`cp`コマンドは古い名前と新しい名前の2つの引数を取ります。
> `gulliver.txt` という名前のファイルのコピーは、どのように作成しますか？試してみてください。
>
> > ## Answer
> > ~~~
> > cp gulliver.txt gulliver-backup.txt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## ディレクトリ名変更
>
> ディレクトリ名の変更処理は、ファイル名の変更と同じ方法になります。
> 試しに `mv` コマンドを使い、`firstdir`というディレクトリを `backup` に変更してみてください。
>
> > ## Answer
> > ~~~
> > mv firstdir backup
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## ファイルをディレクトリの中に移動する。
>
> `mv` コマンドに付ける最後の引数がファイルではなくディレクトリの場合、最初の引数で与えられたファイルはそのディレクトリに移動されます。
> 試しに `mv` コマンドを使い、`gulliver-backup.txt` というファイルを `backup` というフォルダに移動してみてください。
>
> > ## Answer
> > ~~~
> > mv gulliver-backup.txt backup
> > ~~~
> > {: .bash}
> >
> > This would also work:
> >
> > ~~~
> > mv gulliver-backup.txt backup/gulliver-backup.txt
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## ワイルドカードと正規表現
>
> `?`というワイルドカードは、一文字に一致します。`*`というワイルドカードは、ゼロもしくは複数の文字に一致します。
> もし正規表現のレッスンに参加されていたなら、正規表現をどのように表現するか覚えていますか？　
> （正規表現は、シェルの機能ではありませんが、一部のコマンドではサポートしています。これについてはまた後で)
>
> > ## Answer
> > ワイルドカード`?`は、正規表現`.`（ピリオド）に一致します。
> > ワイルドカード`*`は、正規表現`*`に一致します。
> {: .solution}
{: .challenge}

> ## hisroeryコマンドを使う
> hisotoryコマンドを使うことで、最新のセッションで入力したコマンドのリストを見ることができます。
> また、 `Ctrl + r`は逆引きにも使えます。`Ctrl + r`を押してから、探しているコマンドの一部を入力します。過去のコマンドも補完してくれます。
> ` Enter` を押してコマンドを再度実行するか、矢印キーを押してコマンドを編集します。
> 入力した文字列が過去の複数のコマンドに含まれている場合、`Ctrl + r`を繰り返してそれらのコマンドを順に表示させることができます。
> もし逆引きで探しているものが見つからなかったら、`Ctrl + `を使ってプロンプトに戻ります。
> もし履歴を保存し、後でスクリプトを作るためのコマンドを抜き出したいなら、`history > history.txt` で保存できます。
> これによりすべての履歴を `history.txt` というテキストファイルに出力し、後で編集できます。
> 履歴からコマンドを呼び出すには、`history`と入力します。コマンド番号、例えば2045をメモしてください。
> コマンドを呼び出すには、`!2045`と入力します。これでコマンドが実行されます。
{: .challenge}

> ## `echo`コマンドの使用
> `echo` コマンドは、単に指定したテキストを出力するだけです。試してみてください。
> `echo ‘ライブラリカーペントリーはすごい！
> 面白いでしょう？
> 
> 変数を指定することもできます。
> 最初に、`NAME=` と入力し、その後で自分の名前を入力し、Enterキーを押します。
> 次に、`echo "$NAME ライブラリカーペントリーはすごい！"`と入力し、Enterキーを押します。すると、どうなりますか？
>
> テキストと通常のシェルコマンドの両方を、 `echo` を使うことで、今日学んだ `pwd` コマンドのように組み合わせることができます。
> $(` and `)` のように、シェルコマンドを`$(` と `)` で囲みます。
> では、次のように試してみてください。
> > `echo $(date) はよい天気になりました`.
> `date`コマンドの出力は、指定したテキストと一緒に表示されることに注意してください。
> これまでに学んだ他のコマンドで、同じように試してみてください。
> ** echo コマンドは、実際にシェル環境でとても重要だと思いますが、なぜでしょうか？ **
>
> > ## Answer
> > echo` のような基本的なコマンドは、あまり価値がないと思うかもしれません。
> > しかし、自動化されたシェルスクリプトを書き始めた瞬間から、このコマンドは非常に便利なものになります。
> > 例えば、スクリプトの現在の状態など、画面にテキストを出力する必要があることがよくあります。　
> >
> > さらに、最初にシェル変数を使い情報を一時的に保存し、後で再利用することができます。
> > 自動化スクリプトを書き始めると、その機会が多くなるでしょう。
> {: .solution}
{: .challenge}

最後に、削除についてです。
今は使いませんが、何らかの理由でファイルを削除したい場合のコマンドは `rm` または remove です。

ワイルドカードを使うことで、多くのファイルを削除できます。
そして、`-r`フラグを付けることで、フォルダをその内容ごと削除できます。

** グラフィカルユーザーインターフェースからの削除と異なり、警告はなく、ファイルを取り戻すためのごみ箱とか、その他の取り消しオプションもありません。**
ですから、`rm`の扱いには十分注意し、`rm -r`の扱いには細心の注意を払ってください。
