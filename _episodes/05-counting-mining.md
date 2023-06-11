---
title: "シェルを使用した集計とマイニング"
teaching: 60
exercises: 30
questions:
- "データをカウントする"
- "ファイル内のデータを探す"
- "コマンドを組み合わせて新しいことを行う"
objectives:
- "シェル コマンド wc と適切なフラグを使用して、行、単語、および文字を数えるデモを行う"
- "シェルを使用して文字列でファイルを検索し、一致する行を抽出する"
- "シェル コマンドと正規表現を組み合わせてファイルを探索することにより、複雑なコマンドを作成する"
- "コマンドの出力をファイルにリダイレクトする"
- "リダイレクトを使用して、キーボード入力の代わりにファイルを処理する"
- "2つ以上の場面でコマンド パイプラインを構築する"
- "Unixの哲学「一つの大きなシステムを、独立した小さなソフトウェアの集まりとして作るという考え方」を説明する"
keypoints:
- "シェルを使用してドキュメントの要素を集計ができる"
- "シェルを使用して、ファイル内のパターンを検索ができる"
- "コマンドを使用して、任意の数のファイルを集計および探索ができる"
- "コマンドとフラグを組み合わせて、作業のために特別で複雑なクエリを作成できる"

---
##  データの集計とマイニング

シェルの操作方法がわかったところで、次は標準的なシェルコマンドを使ったデータの数え方、調べ方を学びます。
これらのコマンドは、それだけであなたの仕事に革命を起こすことはありませんが、非常に汎用性があり、シェルで作業するための基礎となります。
これらのコマンドは非常に汎用性が高く、シェルで作業するための、そしてコードを学ぶための基礎となるものです。
また、これらのコマンドは、図書館の利用者が図書館のデータを使用する場合の具体例を再現しています。

## 数え方、並べ方

シェルを使ってファイルの中身を数えることから始めます。
シェルを使用すると、ファイル全体から迅速にカウントすることができます。
標準的なオフィススイートのグラフィカルユーザーインターフェースを使用して実現するのは難しいことです。

cdコマンドを使用してデータが格納されているシェルに移動しましょう。

~~~
$ cd shell-lesson
~~~
{: .bash}

もしどこにいるのかわからない場合はpwdコマンドを使ってください。

~~~
$ pwd
~~~
{: .bash}
~~~
/Users/riley/Desktop/shell-lesson
~~~
{: .output}

ディレクトリにどのようなファイルがあり、どのくらいの容量があるのか、`ls -lhS`で確認してみましょう。

~~~
$ ls -lhS
~~~
{: .bash}
~~~
total 139M
-rw-rw-r-- 1 riley staff 126M Jun 10  2015 2014-01_JA.tsv
-rw-r--r-- 1 riley staff 7.4M Jan 31 18:47 2014-01-31_JA-america.tsv
-rw-r--r-- 1 riley staff 3.6M Jan 31 18:47 2014-01-31_JA-africa.tsv
-rw-r--r-- 1 riley staff 1.4M Jan 31 18:47 2014-02-02_JA-britain.tsv
-rw-r--r-- 1 riley staff 598K Jan 31 18:47 gulliver.txt
-rw-r--r-- 1 riley staff 583K Feb  1 22:53 33504-0.txt
drwxr-xr-x 2 riley staff   68 Feb  2 00:58 backup
~~~
{: .output}

このエピソードでは、データセット `2014-01_JA.tsv` に焦点を当てます。
論文のメタデータ、およびオリジナルから派生した 3 つの `.tsv` ファイル
データセットです。これら3つの`.tsv`ファイルには、`2014-01_JA.tsv`の
「Title」フィールドに`africa`や`america`などのキーワードが出現するデータがすべて含まれています。

> ## CSVとTSVファイル
> CSV（Comma-separated values）は、表形式のデータを保存するための一般的なプレーン
> テキスト形式で、各レコードが1行を占め、値はカンマで区切られています。
> TSV (Tab-separated values) is just the same except that values are separated by
> TSV (Tab-separated values) は、値がカンマではなくタブで区切られていることを除けば、
> 同じものです。紛らわしいことに、CSVはCSVとTSVの両方、およびそれらのバリエーション
> を指す言葉として使われることもあります。これらの形式は単純であるため、交換や
> アーカイブに適しています。特定のプログラムに縛られることもなく（例えばExcelファイル
> と違って、CSVというプログラムはなく、Excelを含む多くのプログラムがこの形式をサポート
> しています）、40年前のファイルを今開いても何の問題もありません。
{: .callout}
<!-- ふむ、MARCを思い出す。 -->

まず、学習したツールを使用して、最大のデータファイルを見てみましょう。
[3.ファイルやディレクトリを操作する]({{ page.root }}/03-working-with-files-and-folders/#reading-files):

~~~
$ cat 2014-01_JA.tsv
~~~
{: .bash}

データセット自体が全部表示され、役に立ちません。この進行中のcatコマンドをキャンセル
（あるいはUnixシェルのプロセスをキャンセル）するには、CtlキーとCのキーを押してください。

ほとんどのデータファイルでは、最初の数行を見ただけで、すでに多くのことがわかります。例えば、テーブルやカラムのヘッダーなどです。

~~~
$ head -n 3 2014-01_JA.tsv
~~~
{: .bash}
~~~
File    Creator    Issue    Volume    Journal    ISSN    ID    Citation    Title    Place    Labe    Language    Publisher    Date
History_1a-rdf.tsv  Doolittle, W. E.  1 59  KIVA -ARIZONA-  0023-1940 (Uk)RN001571862 KIVA -ARIZONA- 59(1), 7-26. (1993)  A Method for Distinguishing between Prehistoric and Recent Water and Soil Control Features  xxu eng ARIZONA ARCHAEOLOGICAL AND HISTORICAL SOCIETY 1993
History_1a-rdf.tsv  Nelson, M. C. 1 59  KIVA -ARIZONA-  0023-1940 (Uk)RN001571874 KIVA -ARIZONA- 59(1), 27-48. (1993) Classic Mimbres Land Use in the Eastern Mimbres Region, Southwestern New Mexico xxu eng ARIZONA ARCHAEOLOGICAL AND HISTORICAL SOCIETY 1993

~~~
{: .output}

ヘッダーで学術文献の一般的なメタデータフィールドを見ることができます。`Creator`（著者）、`Issue`（巻号）、`Citation`（出典）などです。

次に、基本的なデータ解析ツールについて学びましょう。
`wc` は `word` をカウントするコマンドで、行数、単語数、バイト数を数えます。
私たちはワイルドカード演算子が好きなので、次のコマンドを実行してみましょう。
`wc *.tsv` というコマンドを実行して、カレントディレクトリにあるすべての `.tsv` ファイルの中身の様々な数を数えてみましょう。ちょっと時間がかかります。

~~~~
$ wc *.tsv
~~~~
{: .bash}
~~~
    13712    511261   3773660 2014-01-31_JA-africa.tsv
    27392   1049601   7731914 2014-01-31_JA-america.tsv
   507732  17606310 131122144 2014-01_JA.tsv
     5375    196999   1453418 2014-02-02_JA-britain.tsv
   554211  19364171 144081136 total
~~~
{: .output}

最初の3列は、行数、ワード数、バイト数です。

比較するファイルが数個しかない場合は、Microsoft ExcelやOpenRefine、
あるいはお気に入りのテキストエディタで確認する方が早いし便利かもしれません。
しかし、数十、数百、数千の文書がある場合、Unixシェルの方が明らかに速度的に有利です。
シェルの本当の威力は、コマンドを組み合わせて、作業を自動化できることです。
このことについて、少し触れておきます。

Fとりあえず、行数の点で最短のファイルを見つける簡単なパイプラインを構築する方法を見てみましょう。
まず、`-l`フラグを追加して、行数だけ取得するようにします。
フラグを追加して、ワード数やバイト数ではなく行数だけを取得します。

~~~~
$ wc -l *.tsv
~~~~
{: .bash}
~~~
    13712 2014-01-31_JA-africa.tsv
    27392 2014-01-31_JA-america.tsv
   507732 2014-01_JA.tsv
     5375 2014-02-02_JA-britain.tsv
   554211 total
~~~
{: .output}

`wc` コマンド自体には出力をソートするフラグがありませんが、これから見るように
3つの異なるシェルコマンドを組み合わせて、欲しいものを手に入れることができます。

まず、`wc -l *.tsv`コマンドがあります。
コマンドの出力を新しいファイルに保存します。
そのためには、コマンドの出力を大なり記号を使って、次のようにファイルにリダイレクトします。

~~~
$ wc -l *.tsv > lengths.txt
~~~
{: .bash}

出力は `lengths.txt` というファイルに入ってしまったので、今は何も出力されていません。
しかし、`cat` や `less` を使って(またはメモ帳などのテキストエディタで) 、
出力が本当にファイルに収まったかどうかを確認することができます。

~~~~
$ cat lengths.txt
~~~~
{: .bash}
~~~
    13712 2014-01-31_JA-africa.tsv
    27392 2014-01-31_JA-america.tsv
   507732 2014-01_JA.tsv
     5375 2014-02-02_JA-britain.tsv
   554211 total
~~~
{: .bash}

次に「sort」コマンドです。 `-n` フラグを使用して、
辞書順ではなく数値順を指定し、結果を別のファイルに出力し、`cat`で
その結果を確認します。

~~~~
$ sort -n lengths.txt > sorted-lengths.txt
$ cat sorted-lengths.txt
~~~~
{: .bash}
~~~
     5375 2014-02-02_JA-britain.tsv
    13712 2014-01-31_JA-africa.tsv
    27392 2014-01-31_JA-america.tsv
   507732 2014-01_JA.tsv
   554211 total
~~~
{: .output}

最後に、古い友人である `head` を使って、 `sorted-lengths.txt` の最初の行を取得します。

~~~~
$ head -n 1 sorted-lengths.txt
~~~~
{: .bash}
~~~
     5375 2014-02-02_JA-britain.tsv
~~~
{: .output}

しかし、私たちが本当に興味があるのは最終結果だけで、`lengths.txt`と`sorted-lengths.txt`に
保存されている中間結果には興味がありません。もし、次のようなことができたらどうでしょう。
最初のコマンド(`wc -l *.tsv`)の出力を次のコマンド(`sort -n`)に引き渡すことができたらどうでしょう。
幸いなことに、パイプという概念を使えば可能です。コマンドライン上では縦棒の文字でパイプを作ります。
まずは一つのパイプで試してみましょう。

~~~~
$ wc -l *.tsv | sort -n
~~~~
{: .bash}
~~~
     5375 2014-02-02_JA-britain.tsv
    13712 2014-01-31_JA-africa.tsv
    27392 2014-01-31_JA-america.tsv
   507732 2014-01_JA.tsv
   554211 
   total
~~~
{: .output}

行数が短いファイルをみつける簡単なパイプラインを作ってみましょう。
-lフラグを使います。tsvファイルの行数がでてきました。wcコマンド自体は出力をsortする機能はありません。

~~~~
$ wc -l *.tsv | sort -n | head -n 1
~~~~
{: .bash}
~~~
     5375 2014-02-02_JA-britain.tsv
~~~
{: .output}

パイプを完全に把握して効率的に使用するには時間がかかる場合がありますが、
これはシェルだけでなく、ほとんどのプログラミング言語にも見られる非常に強力な概念です。

![Redirects and Pipes](../fig/redirects-and-pipes.png)

> ## パイプとフィルター
> このシンプルなアイデアこそが、Unix がこれほどまでに成功した理由です。 
> Unix プログラマーは、多くの異なることをしようとする巨大なプログラムを作成する代わりに、
> それぞれが 1 つの仕事をうまくこなし、互いにうまく連携する単純なツールをたくさん作成することに重点を置いています。
> このプログラミング モデルは「パイプとフィルター」と呼ばれます。パイプはすでに見ました。フィルターは、入力を出力
> に変換する `wc` や `sort` のようなプログラムです。ほとんどすべての標準的な Unix ツールは、このように動作します。
> 特に指示がない限り、標準入力から読み取り、読み取った内容に対して何らかの処理を行い、標準出力に書き込みます。
{: .callout}
<!-- Copied from https://swcarpentry.github.io/shell-novice/04-pipefilter/ -->
> ## パイプの追加
> 私たちは `wc -l *.tsv | sort -n | head -n 1` というパイプを持っています。
> このパイプを `cat` につないだらどうなるでしょうか？試してみてください。
>
> > ## Solution
> > `cat`コマンドは、入力されたものをそのまま出力するので、次のように全く同じ出力が得られます。
> >
> > ~~~
> > $ wc -l *.tsv | sort -n | head -n 1
> > ~~~
> > {: .bash}
> >
> > and
> >
> > ~~~
> > $ wc -l *.tsv | sort -n | head -n 1 | cat
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## カウント、ソート、表示
>すべての`tsv`ファイルの行数を数え、結果をソートして、ファイルの最初の行を表示するには、次のようにします。
>
>~~~
>wc -l *.tsv | sort -n | head -n 1
>~~~
>{: .bash}
>
>
>では、シナリオを変えてみましょう。最も多くの単語を含む10個のファイルを知りたいのです。各ファイルの単語数を数え、順番に並べ、最も単語数の多い10ファイルを出力するために、以下の空欄を埋めてください（ヒント：sortコマンドはデフォルトで昇順にソートします）。
>
>~~~
>__ -w *.tsv | sort __ | ____
>~~~
>{: .bash}
>
> > ## Solution
> >
> > ここでは、`wc`コマンドに `-w` (word) フラグを付けてすべての `tsv` ファイルを `sort` し、`tail` コマンドで最後の 11 行 (10 ファイルと合計) を出力しています。
> >~~~
> > wc -w *.tsv | sort -n | tail -n 11
> >~~~
> >{: .bash}
> {: .solution}
{: .challenge}

> ## ファイル数のカウント
> 別のパイプラインを作成しましょう。現在のディレクトリにあるファイルとディレクトリの
> 数を調べたいとします。 `ls` からの出力を `wc` にパイプして答えを見つけることができる
> かどうかを確認してみてください。
>
> > ## Solution
> > ~~~
> > $ ls | wc -l
> > ~~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## ファイルへの書き込み
> `date`コマンドは、現在の日付と時刻を出力するコマンドです。現在の日付と時刻を 
> `logfile.txt` という新しいファイルに書き込むことができますか？次に、次のことを確認して
> ください。ファイルの内容を表示します。
>
> > ## Solution
> > ~~~
> > $ date > logfile.txt
> > $ cat logfile.txt
> > ~~~~
> > {: .bash}
> > 内容を確認するには、`less`や他の多くのコマンドを使用することもできます。
> >
> > 注意点としては、`>`は警告を出さずに既存のファイルを上書きしてしまうことです。
> > 注意深く行いましょう。
> {: .solution}
{: .challenge}

> ## ファイルに追記する
> `>`はファイルの新規作成を行いますが、`>>`はファイルに追記を行います。現在の日付と
> 時刻を`logfile.txt`に追記してみましょう。
>
> > ## Solution
> > ~~~
> > $ date >> logfile.txt
> > $ cat logfile.txt
> > ~~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## 単語数のカウント
>
> wcコマンドのマニュアルを（man wcまたはwc --helpを使って）チェックして、ワード数
> （行数とバイト数は表示しない）を表示するためのフラグを確認してみてください。.tsv
> ファイルで試してみてください。
>
> もし時間があれば、sortにパイプすることで、結果を並び替えることもできます。また、
> wcの他のオプションも試してみてください。
>
> > ## Solution
> >
> > man wcからは、単語数を表示するための -wフラグがあることがわかります。
> >
> > ~~~
> >      -w      各入力ファイルの単語数が標準出力に書き込まれます。
> > ~~~
> > {: .output}
> >
> > TSVファイルの単語数が表示されます。
> >
> > ~~~
> > $ wc -w *.tsv
> > ~~~
> > {: .bash}
> > ~~~
> >   511261 2014-01-31_JA-africa.tsv
> >  1049601 2014-01-31_JA-america.tsv
> > 17606310 2014-01_JA.tsv
> >   196999 2014-02-02_JA-britain.tsv
> > 19364171 total
> > ~~~
> > {: .output}
> >
> > また、行を数値でソートする場合
> >
> > ~~~
> > $ wc -w *.tsv | sort -n
> > ~~~
> > {: .bash}
> > ~~~
> >   196999 2014-02-02_JA-britain.tsv
> >   511261 2014-01-31_JA-africa.tsv
> >  1049601 2014-01-31_JA-america.tsv
> > 17606310 2014-01_JA.tsv
> > 19364171 total
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}

## マイニングまたは検索

1つまたは複数のファイルから何かを検索することは、しばしば必要になることです。
そこで、それを行うためのコマンドを紹介しましょう。`grep` (**global regular
expression print, グローバル正規表現出力**の略)です。
その名前が示すように、このコマンドは正規表現をサポートしています。
その名の通り、正規表現をサポートしているので、あなたの想像力、データの形状、
そして何千、何百万ものファイルを扱う場合、あなたが自由に使える処理能力によってのみ制限されます。


`grep`を使い始めるには、まだ移動していない場合、まず`shell-lesson`ディレクトリに移動します。
そして、新しいディレクトリresultsを作成してください。
~~~
$ mkdir results
~~~
{: .bash}

では、最初の検索をやってみましょう。

~~~
$ grep 1999 *.tsv
~~~
{: .bash}

シェルは `*.tsv` をディレクトリにあるすべての `.tsv` ファイルのリストに展開しされます。
そして、`grep`はこれらのファイルから "1999"という文字列を検索し、マッチした行を表示します。

> ## 文字列
> 文字列とは、文字の並び、つまり「テキストの一部分」です。
{: .callout}

上矢印キーを1回押して、直近の操作に戻ります。
`grep 1999 *.tsv`を`grep -c 1999 *.tsv`に修正し、Enterキーを押してください。

~~~
$ grep -c 1999 *.tsv
~~~
{: .bash}
~~~
2014-01-31_JA-africa.tsv:804
2014-01-31_JA-america.tsv:1478
2014-01_JA.tsv:28767
2014-02-02_JA-britain.tsv:284
~~~
{: .output}

シェルは、各ファイルに1999という文字列が何回出現したかを表示するようになりました。
前のコマンドの出力を見ると、これは各ジャーナル論文の日付フィールドを参照している
ようです。

別の検索を試してみましょう。

~~~
$ grep -c revolution *.tsv
~~~
{: .bash}
~~~
2014-01-31_JA-africa.tsv:20
2014-01-31_JA-america.tsv:34
2014-01_JA.tsv:867
2014-02-02_JA-britain.tsv:9
~~~
{: .output}

ファイル内の `revolution` という文字列の数が返されました。
さて、先ほどのコマンドを以下のコマンドに修正して、それぞれの出力がどのように異なるかを
観察してみましょう。

~~~
$ grep -ci revolution *.tsv
~~~
{: .bash}
~~~
2014-01-31_JA-africa.tsv:118
2014-01-31_JA-america.tsv:1018
2014-01_JA.tsv:9327
2014-02-02_JA-britain.tsv:122
~~~
{: .output}

これはクエリーを繰り返すものですが、大文字と小文字を区別せずにカウントを表示します。
(`revolution`と`Revolution`の両方、あるいはその派生（訳注: ”revolutions”や”revolutionary”など）
を含みます。)

「america」というキーワードを含むジャーナル記事のタイトルが、30倍近くに増えていることに注目してください。
前回と同様に、>results/とファイル名（.txt形式が理想的）を追加すると、結果をデータファイルに保存することができます。

~~~
$ grep -i revolution *.tsv
~~~
{: .bash}

このスクリプトは、定義されたファイルを調べて、`revolution` を含む行を
(大文字小文字を区別せずに) シェルに表示します。
シェルに、今日の日付をファイル名に追加させます。
~~~
$ grep -i revolution *.tsv > results/$(date "+%Y-%m-%d")_JAi-revolution.tsv
~~~
{: .bash}

抽出されたデータを新しいファイルに保存します。

> ## dateの代替コマンド
> このような日付の書き方は非常に一般的で、一部のプラットフォーム（macOS X以外）
> では、`$(date "%Y-%m-%d")` の代わりに `$(date -I)` と入力しても同じ結果を
> 得ることができます。
{: .callout}

しかし、このファイルを見ると、'revolution'という文字列が、'revolutionary'などの
他の単語の一部も含めて出力されています。
これは思ったほど便利ではないかもしれません...。
ありがたいことに、`-w` フラグは `grep` に単語全体のみを検索するように指示します。
これによって、より正確に検索することができます。

~~~
$ grep -iw revolution *.tsv > results/$(date "+%Y-%m-%d")_JAiw-revolution.tsv
~~~
{: .bash}

このスクリプトは、定義された両方のファイルを検索し、大文字・小文字を区別せず、`revoltion`
という単語を含む行すべてを、指定された `.tsv` ファイルに出力します。
作成したファイルの差分を表示することができます。

~~~
$ wc -l results/*.tsv
~~~
{: .bash}
~~~
   10585 2016-07-19_JAi-revolution.tsv
    7779 2016-07-19_JAiw-revolution.tsv
   18364 total
~~~
{: .output}

> ## 日付を自動的に追加する
> 今日の日付を自分で入力しなかったことに注意してください。
> `date` コマンドはそんなつまらない仕事をしてくれます。
> `"+%Y-%m-%d"` オプションと、使用可能な代替オプションについて調べてみましょう。
>
> > ## Solution
> > `date --help` を使用すると、`+` オプションが導入されていることがわかります
> > `%Y`、`%m`、`%d` はそれぞれ年、月、日に置き換えられます。
> > 他にもたくさんのパーセントコードがあります
> > `-I` は[--iso-8601](https://en.wikipedia.org/wiki/ISO_8601)の省略形で、
> >ヨーロッパの日付形式`DD.MM.YYYY` と
> > アメリカの日付形式  `MM/DD/YYYY`の違いによる混乱を避けるためのオプションです。
> {: .solution}
{: .challenge}

最後に、先ほど取り上げた**正規表現構文**を使って、似たような単語を検索してみます。

> ## 基本正規表現、拡張正規表現、Perl互換正規表現
> 残念ながら、正規表現には書き方の違いがあります。
> 様々なバージョンにおいて、`grep`は「基本正規表現」、少なくとも2種類の拡張表現である
> 「拡張正規表現」と「Perl互換正規表現」をサポートしています。私たちの
> チュートリアルを含め、ほとんどのチュートリアルではPerlプログラミング言語と互換性の
> ある正規表現を教えていますが、`grep`はデフォルトで基本正規表現
> を使用するので、これはよくある混乱の原因となっています。
> 細かいことを覚えておきたいのでなければ、常にあなたのバージョンの `grep` がサポートする
> 最も高度な正規表現を使用するのが簡単です。(`-E` フラグを使用します。macOS X、
> 他のほとんどのプラットフォームでは `-P`） または、単純な文字列検索よりも複雑なことをするときに使用します。
{: .callout}

正規表現'fr[ae]nc[eh]'は、「france」「french」の他に、「frence」「franch」にもマッチします。
一般的には、式をシングルクォーテーションで囲むとよいでしょう、
というのは、シェルがワイルドカード演算子を展開しようとするような処理をせずに、直接grepに送信することを保証するためです。

~~~
$ grep -iwE 'fr[ae]nc[eh]' *.tsv
~~~
{: .bash}

シェルは一致する行をそれぞれ出力します。

マッチした行だけを表示するために `-o` フラグを指定します。
(結果の切り分けやチェックに便利です)

~~~
$ grep -iwEo 'fr[ae]nc[eh]' *.tsv
~~~
{: .bash}

近くの人とペアを組んで、これらの課題に取り組んでみてください。

> ## 大文字小文字を区別して検索
> このディレクトリにある4つの派生ファイル `.tsv` から、
> 選択した単語全体の大文字と小文字を区別して検索してください。
> また、結果をシェルに出力してください。
>
> > ## Solution
> > ~~~
> > $ grep -w hero *.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## 選択したファイルの大文字と小文字を区別して検索する
> このディレクトリにある 'America' と 'Africa' の `.tsv` ファイルから、大文字と小文字を区別して選択した単語をすべて検索してください。
> 大文字と小文字を区別して選択した単語をすべて検索してください。
> また、結果をシェルに出力してください。
>
> > ## Solution
> > ~~~
> > $ grep hero *a.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## 語数を数える（大文字と小文字を区別する）
> このディレクトリにある 'America' と 'Africa' の `.tsv` ファイルで、
> 選んだ単語の大文字と小文字を区別するすべての単語をカウントしてください。
> また、結果をシェルに出力してください。
>
> > ## Solution
> > ~~~
> > $ grep -c hero *a.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## 単語を数える（大文字・小文字を区別しない）。
> 「America」と「Africa」の `.tsv` ファイルに含まれる、大文字と小文字を区別せずに
> すべての単語をカウントするようこのディレクトリで実行してください。また、結果をシェルに出力してください。
>
> > ## Solution
> > ~~~
> > $ grep -ci hero *a.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Case insensitive search in select files
> Search for all case insensitive instances of that
> word in the 'America' and 'Africa' `.tsv` files in this directory. Print your results to  a file `results/hero.tsv`.
>
> > ## Solution
> > ~~~
> > $ grep -i hero *a.tsv > results/hero.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Case insensitive search in select files (whole word)
> Search for all case insensitive instances of that whole word
> in the 'America' and 'Africa' `.tsv` files in this directory. Print your results to a file `results/hero-i.tsv`.
>
> > ## Solution
> > ~~~
> > $ grep -iw hero *a.tsv > results/hero-i.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Searching with regular expressions
> Use regular expressions to find all ISSN numbers
> (four digits followed by hyphen followed by four digits)
> in `2014-01_JA.tsv` and print the results to a file `results/issns.tsv`.
> Note that you might have to use the `-E` flag (or `-P` with some versions
> of `grep`, e.g. with Git Bash on Windows).
>
> > ## Solution
> > ~~~
> > $ grep -Eo '\d{4}-\d{4}' 2014-01_JA.tsv > results/issns.tsv
> > ~~~
> > {: .bash}
> >
> > or
> >
> > ~~~
> > $ grep -Po '\d{4}-\d{4}' 2014-01_JA.tsv > results/issns.tsv
> > ~~~
> > {: .bash}
> >
> > It is worth checking the file to make sure `grep` has interpreted the pattern
> > correctly. You could use the `less` command for this.
> >
> > The `-o` flag means that only the ISSN itself is printed out, instead of the
> > whole line.
> >
> > If you came up with something more advanced, perhaps including word boundaries,
> > please share your result in the collaborative document and give yourself a pat on the shoulder.
> >
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Finding unique values
> If you pipe something to the `uniq` command, it will filter out adjacent duplicate lines.
> In order for the 'uniq' command to only return unique values though, it needs to be used
> with the 'sort' command. Try piping the output from the command in the last exercise
> to `sort` and then piping these results to 'uniq' and then `wc -l` to count the number of unique ISSN values.
>
> > ## Solution
> > ~~~
> > $ grep -Eo '\d{4}-\d{4}' 2014-01_JA.tsv | sort | uniq | wc -l
> > ~~~
> > {: .bash}
> >
> > or
> > ~~~
> > $ grep -Po '\d{4}-\d{4}' 2014-01_JA.tsv | sort | uniq | wc -l
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

### Using a Loop to Count Words

We will now use a loop to automate the counting of certain words within a document. For this, we will be using the _[Little Women](http://www.gutenberg.org/cache/epub/514/pg514.txt)_ e-book from [Project Gutenberg](https://www.gutenberg.org/). The file is inside the `shell-lesson` folder and named `pg514.txt`. Let's rename the file to `littlewomen.txt`. 

~~~
$ mv pg514.txt littlewomen.txt
~~~

This renames the file to something easier to type.

Now let's create our loop. In the loop, we will ask the computer to go through the text, looking for each girl's name,
and count the number of times it appears. The results will print to the screen.

~~~
$ for name in "Jo" "Meg" "Beth" "Amy"
> do
>    echo "$name"
>    grep -wo "$name" littlewomen.txt | wc -l
> done
~~~

{: .bash}

~~~
Jo
1355
Meg
683
Beth
459
Amy
645
~~~
{: .output}

What is happening in the loop?  
+ `echo "$name"` is printing the current value of `$name`
+ `grep "$name" littlewomen.txt` finds each line that contains the value stored in `$name`. The `-w` flag finds only the whole word that is the value stored in `$name` and the `-o` flag pulls this value out from the line it is in to give you the actual words to count as lines in themselves.
+ The output from the `grep` command is redirected with the pipe, `|` (without the pipe and the rest of the line, the output from `grep` would print directly to the screen)
+ `wc -l` counts the number of _lines_ (because we used the `-l` flag) sent from `grep`. Because `grep` only returned lines that contained the value stored in `$name`, `wc -l` corresponds to the number of occurrences of each girl's name.

> ## Why are the variables double-quoted here?
>
> a) In [episode 4]({{ page.root }}{% link _episodes/04-loops.md %}) we learned to
> use `"$..."` as a safeguard against white-space being misinterpreted.
> Why _could_ we omit the `"`-quotes in the above example?
> 
> b) What happens if you add `"Louisa May Alcott"` to the first line of
> the loop and remove the `"` from `$name` in the loop's code?
> 
>> ## Solutions
>> 
>> a) Because we are explicitly listing the names after `in`,
>> and those contain no white-space. However, for consistency
>> it's better to use rather once too often than once too rarely.
>> 
>> b) Without `"`-quoting `$name`, the last loop will try to execute
>> `grep Louisa May Alcott littlewomen.txt`. `grep` interprets only the
>> first word as the search pattern, but `May` and `Alcott` as filenames.
>> This produces two errors and a possibly untrustworthy count:
>> ~~~
>> ...
>> Louisa May Alcott
>> grep: May: No such file or directory
>> grep: Alcott: No such file or directory
>> 4
>> ~~~
>> {: .bash}
> {: .solution}
{: .challenge}

> ## Selecting columns from our article dataset
> When you receive data it will often contain more columns or variables than you need for your work. If you want to select only the columns you need for your analysis, you can use the `cut` command to do so. `cut` is a tool for extracting sections from a file. For instance, say we want to retain only the `Creator`, `Volume`, `Journal`, and `Citation` columns from our article data. With `cut` we'd:
>~~~
> cut -f 2,4,5,8 2014-01_JA.tsv | head
>~~~
>{: .bash}
>
>~~~
> Creator	Volume	Journal	Citation
> Doolittle, W. E.  59  KIVA -ARIZONA-  KIVA -ARIZONA- 59(1), 7-26. (1993)
> Nelson, M. C.	59	KIVA -ARIZONA-	KIVA -ARIZONA- 59(1), 27-48. (1993)
> Deegan, A. C.	59	KIVA -ARIZONA-	KIVA -ARIZONA- 59(1), 49-64. (1993)
> Stone, T.	59	KIVA -ARIZONA-	KIVA -ARIZONA- 59(1), 65-82. (1993)
> Adams, W. Y.	1	NORTHEAST AFRICAN STUDIES	NORTHEAST AFRICAN STUDIES 1(2/3), 7-18. (1994)
> Beswick, S. F.	1	NORTHEAST AFRICAN STUDIES	NORTHEAST AFRICAN STUDIES 1(2/3), 19-48. (1994)
> Cheeseboro, A. Q.	1	NORTHEAST AFRICAN STUDIES	NORTHEAST AFRICAN STUDIES 1(2/3), 49-74. (1994)
> Duany, W.	1	NORTHEAST AFRICAN STUDIES	NORTHEAST AFRICAN STUDIES 1(2/3), 75-102. (1994)
> Mohamed Ibrahim Khalil	1	NORTHEAST AFRICAN STUDIES	NORTHEAST AFRICAN STUDIES 1(2/3), 103-118. (1994)
>~~~
>{: .output}
>
> Above we used `cut` and the `-f` flag to indicate which columns we want to retain. `cut` works on tab delimited files by default. We can use the flag `-d` to change this to a comma, or semicolon or another delimiter.
> If you are unsure of your column position and the file has headers on the first line, we can use `head -n 1 <filename>` to print those out.
> ### Now your turn
>Select the columns `Issue`, `Volume`, `Language`, `Publisher` and direct the output into a new file. You can name it something like `2014-01_JA_ivlp.tsv`.
>> ## Solution
>> First, let's see where our desired columns are:
>>
>>~~~
>> head -n 1 2014-01_JA.tsv
>>~~~
>>{: .bash}
>>
>>~~~
>>File	Creator	Issue	Volume	Journal	ISSN	ID	Citation	Title	Place Labe	Language	Publisher	Date
>>~~~
>>{: .output}
>>
>>Ok, now we know `Issue` is column 3, `Volume` 4, `Language` 11, and `Publisher` 12.
>> We use these positional column numbers to construct our `cut` command:
>>```
>> cut -f 3,4,11,12 2014-01_JA.tsv > 2014-01_JA_ivlp.tsv
>>```
>> We can confirm this worked by running head on the file:
>>```
>>head 2014-01_JA_ivlp.tsv
>>```
>>~~~
>>Issue	Volume	Language	Publisher
>>1	59	eng	ARIZONA ARCHAEOLOGICAL AND HISTORICAL SOCIETY
>>1	59	eng	ARIZONA ARCHAEOLOGICAL AND HISTORICAL SOCIETY
>>1	59	eng	ARIZONA ARCHAEOLOGICAL AND HISTORICAL SOCIETY
>>1	59	eng	ARIZONA ARCHAEOLOGICAL AND HISTORICAL SOCIETY
>>~~~
>>{: .output}
> {: .solution}
{: .challenge}
