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
   554211 total
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

> ## Appending to a file
> While `>` writes to a file, `>>` appends something to a file. Try to append the
> current date and time to the file `logfile.txt`?
>
> > ## Solution
> > ~~~
> > $ date >> logfile.txt
> > $ cat logfile.txt
> > ~~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Counting the number of words
>
> Check the manual for the `wc` command (either using `man wc` or `wc --help`)
> to see if you can find out what flag to use to print out the number of words
> (but not the number of lines and bytes). Try it with the `.tsv` files.
>
> If you have time, you can also try to sort the results by piping it to `sort`.
> And/or explore the other flags of `wc`.
>
> > ## Solution
> >
> > From `man wc`, you will see that there is a `-w` flag to print the number of
> > words:
> >
> > ~~~
> >      -w      The number of words in each input file is written to the standard
> >              output.
> > ~~~
> > {: .output}
> >
> > So to print the word counts of the `.tsv` files:
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
> > And to sort the lines numerically:
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

## Mining or searching

Searching for something in one or more files is something we'll often need to do,
so let's introduce a command for doing that: `grep` (short for **global regular
expression print**). As the name suggests, it supports regular expressions and
is therefore only limited by your imagination, the shape of your data, and - when
working with thousands or millions of files - the processing power at your disposal.

To begin using `grep`, first navigate to the `shell-lesson` directory if not already
there. Then create a new directory "results":
Grepを使う。想像力次第で思い通りにできる
移動しましょう。リザルトディレクトリを作ります。
~~~
$ mkdir results
~~~
{: .bash}


Now let's try our first search:

~~~
$ grep 1999 *.tsv
~~~
{: .bash}

Remember that the shell will expand `*.tsv` to a list of all the `.tsv` files in the
directory. `grep` will then search these for instances of the string "1999" and
print the matching lines.

ここでいうstring テキストの固まり

> ## Strings
> A string is a sequence of characters, or "a piece of text".
{: .callout}


Press the up arrow once in order to cycle back to your most recent action.
Amend `grep 1999 *.tsv` to `grep -c 1999 *.tsv` and hit enter.

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

The shell now prints the number of times the string 1999 appeared in each file.
If you look at the output from the previous command, this tends to refer to the
date field for each journal article.

大文字小文字を区別しない


We will try another search:

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

We got back the counts of the instances of the string `revolution` within the files.
Now, amend the above command to the below and observe how the output of each is different:

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

This repeats the query, but prints a case
insensitive count (including instances of both `revolution` and `Revolution` and other variants).
Note how the count has increased nearly 30 fold for those journal article
titles that contain the keyword 'america'. As before, cycling back and
adding `> results/`, followed by a filename (ideally in .txt format), will save the results to a data file.

So far we have counted strings in files and printed to the shell or to
file those counts. But the real power of `grep` comes in that you can
also use it to create subsets of tabulated data (or indeed any data)
from one or multiple files.  

~~~
$ grep -i revolution *.tsv
~~~
{: .bash}

This script looks in the defined files and prints any lines containing `revolution`
(without regard to case) to the shell. We let the shell add today's date to the
filename:

~~~
$ grep -i revolution *.tsv > results/$(date "+%Y-%m-%d")_JAi-revolution.tsv
~~~
{: .bash}

This saves the subsetted data to a new file.

> ## Alternative date commands
> This way of writing dates is so common that on some platforms (not macOS X)
> you can get the same result by typing `$(date -I)` instead of
> `$(date "+%Y-%m-%d")`.
{: .callout}

However, if we look at this file, it contains every instance of the
string 'revolution' including as a single word and as part of other words
such as 'revolutionary'. This perhaps isn't as useful as we thought...
Thankfully, the `-w` flag instructs `grep` to look for whole words only,
giving us greater precision in our search.

~~~
$ grep -iw revolution *.tsv > results/$(date "+%Y-%m-%d")_JAiw-revolution.tsv
~~~
{: .bash}

This script looks in both of the defined files and
exports any lines containing the whole word `revolution` (without regard to case)
to the specified `.tsv` file.

We can show the difference between the files we created.

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

> ## Automatically adding a date prefix
> Notice how we didn't type today's date ourselves, but let the
> `date` command do that mindless task for us. Find out about the
> `"+%Y-%m-%d"` option and alternative options we could have used.
>
> > ## Solution
> > Using `date --help` will show you that the `+` option introduces
> > a date format, where `%Y`, `%m` and `%d` are replaced by the year,
> > month, and day respectively. There are many other percent-codes
> > you could use.
> >
> > You might also see that `-I` is short for
> > [--iso-8601](https://en.wikipedia.org/wiki/ISO_8601), which
> > essentially avoids the confusion between the European
> > and American date formats `DD.MM.YYYY` and `MM/DD/YYYY`.
> {: .solution}
{: .challenge}

オプションを探してみましょう。dateのヘルプをみると

Finally, we'll use the **regular expression syntax** covered earlier to search for similar words.

> ## Basic, extended, and PERL-compatible regular expressions
> There are, unfortunately, [different ways of writing regular expressions](https://www.gnu.org/software/grep/manual/html_node/Regular-Expressions.html).
> Across its various versions, `grep` supports "basic", at least two types of "extended",
> and "PERL-compatible" regular expressions. This is a common cause of confusion, since
> most tutorials, including ours, teach regular expressions compatible with the PERL
> programming language, but `grep` uses basic by default.
> Unless you want to remember the details, make your life easy by always using the
> most advanced regular expressions your version of `grep` supports (`-E` flag on
> macOS X, `-P` on most other platforms) or when doing something more complex
> than searching for a plain string.
{: .callout}

The regular expression 'fr[ae]nc[eh]' will match "france", "french", but also "frence" and "franch".
It's generally a good idea to enclose the expression in single quotation marks, since
that ensures the shell sends it directly to grep without any processing (such as trying to
expand the wildcard operator *).

~~~
$ grep -iwE 'fr[ae]nc[eh]' *.tsv
~~~
{: .bash}

The shell will print out each matching line.

We include the `-o` flag to print only the matching part of the lines e.g.
(handy for isolating/checking results):

~~~
$ grep -iwEo 'fr[ae]nc[eh]' *.tsv
~~~
{: .bash}

Pair up with your neighbor and work on these exercises:

> ## Case sensitive search
> Search for all case sensitive instances of
> a whole word you choose in all four derived `.tsv` files in this directory.
> Print your results to the shell.
>
> > ## Solution
> > ~~~
> > $ grep -w hero *.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Case sensitive search in select files
> Search for all case sensitive instances of a word you choose in
> the 'America' and 'Africa' `.tsv` files in this directory.
> Print your results to the shell.
>
> > ## Solution
> > ~~~
> > $ grep hero *a.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Count words (case sensitive)
> Count all case sensitive instances of a word you choose in
> the 'America' and 'Africa' `.tsv` files in this directory.
> Print your results to the shell.
>
> > ## Solution
> > ~~~
> > $ grep -c hero *a.tsv
> > ~~~
> > {: .bash}
> {: .solution}
{: .challenge}

> ## Count words (case insensitive)
> Count all case insensitive instances of that word in the 'America' and 'Africa' `.tsv` files
> in this directory. Print your results to the shell.
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
