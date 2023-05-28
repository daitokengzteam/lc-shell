---
title: "フリーテキストの操作"
teaching: 20
exercises: 40
questions:
- "複雑なファイルをどのように操作しますか？"
objectives:
- "シェルのツールを使い、フリーテキストのクリーニングと変換を行う"
keypoints:
- "シェルツールを組み合わせて、強力な効果を発揮"
---
### フリーテキストでの作業

これまで、Unixシェルを使い、表データの操作、数えたり、マイニングするの方法をみてきました。
図書館のデータ、特にデジタル化された文書には、表形式のメタデータよりもずっと厄介なものがあります。
それでも、同じ手法の多くは、フリーテキストのような表になっていない非集計データに適用することができまする。
何を数えるのかとや、UNIXシェルでのベストな方法を注意深く考える必要があります。
ありがたいことに、この種の仕事をしている人がたくさんいて、より複雑なファイルを扱うための入門として、彼らがしていることを借用することができます。
それで、最後の演習では、難易度を少し上げ、起きていることすべての詳細や、各コマンドについてじっくり議論したりすることのないシナリオにします。
テキストを準備したり、分解したりして、Unixシェルの潜在的なアプリケーションのいくつかを示すことにします。
学習したコマンドを使用するところでは、一部説明を省いてあり、行き詰まったらノートを参照してください。
先に進む前に、隣の人に声をかけて、どのタイプの文章を一緒に作るか選んでください。選択肢は3つです、

- 写本のテキストの例：ガリバー旅行記(1735)
- OCRで取り込まれたテキストの例：メリーランドの地形に関する一般報告：学位論文（Report of Mayland State Weathからの転載、地図・イラストを含む）[https://doi.org/10.21250/db12](https://doi.org/10.21250/db12)
- Webページの例；パイパーの世界 （1999年にarchive.orgに保存されたGeoCitiesのホームページ）(http://wayback.archive.org/web/20091020080943/http:/geocities.com/Heartland/Hills/7649/diary.html))

## オプション１：写本
### テキストを集めて、整形する。
前回のレッスンで作成した `gulliver.txt` ファイルを使い作業を進めます。
shell-lessonディレクトリで作業します。
ファイルを見てみましょう。

~~~
$ less -N gulliver.txt
~~~
{: .bash}
~~~
      1 <U+FEFF>The Project Gutenberg eBook, Gulliver's Travels, by Jonatha
      1 n Swift
      2
      3
      4 This eBook is for the use of anyone anywhere at no cost and with
      5 almost no restrictions whatsoever.  You may copy it, give it away o
      5 r
      6 re-use it under the terms of the Project Gutenberg License included
      7 with this eBook or online at www.gutenberg.org
      8
      9
     10
     11
     12
     13 Title: Gulliver's Travels
     14        into several remote nations of the world
     15
     16
     17 Author: Jonathan Swift
     18
     19
     20
     21 Release Date: June 15, 2009  [eBook #829]
     22
     23 Language: English
     24
     25 Character set encoding: UTF-8
     26
     27
     28 ***START OF THE PROJECT GUTENBERG EBOOK GULLIVER'S TRAVELS***
     29
     30
     31 Transcribed from the 1892 George Bell and Sons edition by David Pri
     31 ce,
     32 email ccx074@pglaf.org
     33
     34
     35
~~~
{: .output}

`sed` コマンドを使うことから始めていきます。このコマンドは、ファイルを直接編集できます。

~~~
$ sed '9352,9714d' gulliver.txt > gulliver-nofoot.txt
~~~
{: .bash}

コマンドの `sed` に`d`の値を組み合わせると、 `gulliver.txt` を見て、指定した行の間のすべての値を削除します。
'>` のアクションは、編集されたテキストを指定された新しいファイルに出力するようスクリプトに促します。

~~~
$ sed '1,37d' gulliver-nofoot.txt > gulliver-noheadfoot.txt
~~~
{: .bash}

同じことをヘッダーにも行います。
テキストをきれいにできました。
trというコマンドを使い、文字を変換したり削除します。入力し、実行してください。

~~~
$ tr -d '[:punct:]\r' < gulliver-noheadfoot.txt > gulliver-noheadfootpunct.txt
~~~
{: .bash}

これは、translateコマンドを使い、すべての句読点(`[:punct:]`)とキャリッジリターンを削除する特別な構文です。
これまで見てきた出力リダイレクト `>` とまだ見ていない入力リダイレクト `<` の両方の使用が求められます。
最後に、大文字をすべて小文字に変換し、テキストを正規化します。


~~~
$ tr [:upper:] [:lower:] < gulliver-noheadfootpunct.txt > gulliver-clean.txt
~~~
{: .bash}

テキストエディタで`gulliver-clean.txt`を開いてください。
テキストがどのように変換され、分析の準備が整ったかみてください。

### テキストを分解し、単語の出現頻度を数える
これで、テキストを分解する準備が整いました。

~~~
$ tr ' ' '\n' < gulliver-clean.txt | sort | uniq -c | sort -nr > gulliver-final.txt
~~~
{: .bash}

ここでは、[シェルのカウントとマイニング]で見たパイプを拡張して使います。
このスクリプトの最初のところでは、再び translate コマンドを使い、すべての空白を \`n` に変換し、新しい行として表示します。この段階で、ファイル内のすべての単語が一行ごとになります。
第二段階では、`sort`コマンドを使用し、テキストを元の順序からアルファベット順に並べ替えます。
第三段階では、他の新しいコマンドの `uniq` と `-c` フラグを組み合わせ、重複行を削除し、重複している文字数を出力します。
最後の第四段階では、3つ目のステップで生成された重複をカウントしたものを再度ソートします。

## 課題
本文中には、まだ句読点が残っています。それらは「スマートクォート」又は「カーリークォート」と呼ばれます。
sed` を使って削除できますか？

ヒント：引用符は、ASCII規格の128文字にありません、それで、ファイルの中では別の規格であるUTF-8でエンコードされています
これは `sed` では問題ありませんが、入力する端末のウィンドウは UTF-8 を理解できないかもしれません。
その場合、Bashスクリプトを使用する必要があります：Bashスクリプトは、エピソード4「ループを使った面倒なことの自動化」の最後のところで触れています。
回数を出力できるようになりました。これは私達が調査の基本として、他のテキストと比べることができる。違うデータの変化を試したい場合は最初から処理をします。
注意点として、お好みのテキストエディタを使い、以下のようなファイルを書いてください。

> > ```
> > #!/bin/bash
> > # このスクリプトは、gulliver-clean.txtから引用符を削除し、その結果をgulliver-noquotes.txtとして保存します。
> > (replace this line with your solution)
> > ```
> > {: .bash}
> Save the file as `remove-quotes.sh` and run it from the command line like this:
> > ```
> > bash remove-quotes.sh
> > ```
> > {: .bash}
>
> > ## Solution
> > ```
> > #!/bin/bash
> > # This script removes quote marks from gulliver-clean.txt and saves the result as gulliver-noquotes.txt
> > sed -Ee 's/[“”‘’]//g' gulliver-clean.txt > gulliver-noquotes.txt
> > ```
> > {: .bash}
> > If this doesn't work for you, you might need to check whether your text editor can
> > save files using the UTF-8 encoding.
> {: .solution}
{: .challenge}

今、私たちはテキストを分解し、その中のそれぞれの単語の個数を数えました。
これは、突合して可視化できるデータで、同じ方法で処理された別のテキストと比較することができます。
また、異なる方法で別の変換を実行する必要がある場合は、`gulliver-clean.txt` に戻り、その作業を開始することができます。
これらのすべては、地味でも非常に強力なコマンドライン上のいくつかのコマンドを使っています。

## オプション2：光学文字認識テキスト
### テキストを集め、きれいにする
`201403160_01_text.json`で作業を進めていきます。
ファイルを見ましょう。

~~~
$ less -N 201403160_01_text.json
~~~
{: .bash}
~~~
      1 [[1, ""], [2, ""], [3, ""], [4, ""], [5, ""], [6, ""], [7, "A GENERAL RE
      1 PORT ON THE PHYSIOGRAPHY OF MARYLAND A DISSERTATION PRESENTED TO THE PRE
      1 SIDENT AND FACULTY OF THE JOHNS HOPKINS UNIVERSITY FOR THE DEGREE OF DOC
      1 TOR OF PHILOSOPHY BY CLEVELAND ABBE, Jr. BALTIMORE, MD. MAY, 1898."], [8
      1 , ""], [9, ""], [10, "A MAP S H OW I N G THE PHYSIOGRAPHIC PROVINCES OF
      1 MARYLAND AND Their Subdivisions Scale 1 : 2,000.000. 32 Miles-1 Inch"],
      1 [11, "A GENERAL REPORT ON THE PHYSIOGRAPHY OF MARYLAND A DISSERTATION PR
      1 ESENTED TO THE PRESIDENT AND FACULTY OF THE JOHNS HOPKINS UNIVERSITY FOR
      1  THE DEGREE OF DOCTOR OF PHILOSOPHY BY CLEVELAND ABBE, Jr. BALTIMORE, MD
      1 . MAY, 1898."], [12, "PRINTED BY tL%t jfricbcnrtxifti Compang BALTIMORE,
      1  MD., U. S. A. REPRINTED FROM Report of Maryland State Weather Service,
      1 Vol. 1, 1899, pp. 41-216."], [13, "A GENERAL REPORT ON THE PHYSIOGRAPHY
      1 OF MARYLAND Physiographic Processes. INTRODUCTION. From the earliest tim
      1 es men have observed more or less closely the various phenomena which na
      1 ture presents, and have sought to find an explanation for them. Among th
      1 e most interesting of these phe nomena have been those which bear on the
      1  development of the sur face features of the earth or its topography. Im
      1 pressed by the size and grandeur of the mountains, their jagged crests a
      1 nd scarred sides, early students of geographical features were prone to
      1 ascribe their origin to great convulsions of the earth's crust, earthqua
      1 kes and vol canic eruptions. One generation after another comes and goes
      1 , yet the mountains continue to rear their heads to the same heights, th
      1 e rivers to run down the mountain sides in the same courses and follow t
~~~
{: .output}

文字の解釈や削除で使う `tr` コマンドを使うことから始ます。
タイプして実行します。

~~~
$ tr -d [:punct:] < 201403160_01_text.json > 201403160_01_text-nopunct.txt
~~~
{: .bash}

こちらは、translateコマンドと特殊な構文を使い、すべての句読点を削除するものです。
これまで見てきた出力リダイレクト `>` とまだ見ていない入力リダイレクト `<` の両方を使用する必要があります。
最後に、大文字をすべて削除して、テキストを正規化します。

~~~
$ tr [:upper:] [:lower:] < 201403160_01_text-nopunct.txt > 201403160_01_text-clean.txt
~~~
{: .bash}

201403160_01_text-clean.txt` というファイルをテキストエディタで開いてください。
テキストがどのように変換され、分析できるようになったかに注目してください。

### テキストを分解し、単語頻度を数える
ここで、テキストを取り出し、分解する準備が整いました。

~~~
$ tr ' ' '\n' < 201403160_01_text-clean.txt | sort | uniq -c | sort -nr > 201403160_01_text-final.txt
~~~
{: .bash}

Here we've made extended use of the pipes we saw in [Counting and mining with the shell]({{ page.root }}{% link _episodes/05-counting-mining.md %}). 
ここでは、[シェルを使用した集計とマイニング]({{ page.root }}{% link _episodes/05-counting-mining.md %})で見たパイプを拡張して使います。
このスクリプトの最初の部分では、再び translate コマンドを使い、すべての空白を `\n` に変換し、改行として表示します。
この段階で、ファイル内の単語がそれぞれの行を持つことになります。
次に、`sort`コマンドを使い、テキストを元の並びからアルファベット順に並べ替えます。
次に、もう一つの新しいコマンドの `uniq` に `-c` フラグを組み合わせて、重複した行を削除し、単語数をカウントします。
次に、前のステップで生成された単語数によって、再びテキストを並べ替えます。

**注：最終的な出力には1つの問題があります、数えられた単語がすべて本当の単語であるとは限りません（1回または2回だけ数えられた単語を参照）。
何が起こったのかをよく理解するためには、ネットで「OCR 認識精度」」を検索してください。**

いずれにせよ、これでテキストを分解して、その中の単語を一つ一つ数えることができるようになりました。
これは、突合し可視化できるデータで、調査の基礎となる様式となるものであり、同じ方法で処理された別のテキストと比較することができます。
別の分析のために別の変換セットを実行する必要がある場合は、`201403160_01_text-clean.txt`に戻り、その作業を開始することができます。
これらすべては、地味ではあるが非常に強力なコマンドライン上のいくつかのコマンドを使います。

## オプション3：ウェブページ

### テキストを集め、綺麗にする

`diary.html`で作業を進めます。
ファイルを見てみましょう。

~~~
$ less -N diary.html
~~~
{: .bash}
~~~
      1 <!-- This document was created with HomeSite v2.5 -->
      2 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2//EN">
      3 <html>
      4
      5 <head>
      6
      7
      8 <script type="text/javascript" src="/static/js/analytics.js"></script>
      9 <script type="text/javascript">archive_analytics.values.server_name="www
      9 b-app6.us.archive.org";archive_analytics.values.server_ms=105;</script>
     10 <link type="text/css" rel="stylesheet" href="/static/css/banner-styles.c
     10 ss"/>
     11
     12
     13 <title>Piper's Diary</title>
     14 <meta name="description"
     15 content="Come visit our shih tzu, Piper.  He has his very own photo gall
     15 ary, monthly diary, newsletter, and dog site award.  He also maintains d
     15 og book reviews and quotations.  Come check him out!">
     16 <meta name="keywords"
     17 content="shih tzu, dog, pet, quotations, award, diary, advice, book, rev
     17 iew, piper">
     18 <style TYPE="text/css" TITLE="Basic Fonts">

~~~
{: .output}

`sed`コマンドを使い進めていきます。
このコマンドは、ファイルを直接編集できます。

~~~
$ sed '265,330d' diary.html > diary-nofoot.txt
~~~
{: .bash}

コマンド `sed` に`d` を組み合わせ、`diary.html` を見て、 特定行の間のすべての値を削除します。
'>'のアクションは、編集されたテキストを特定の新しいファイルに保存するようスクリプトにさせるものです。

~~~
$ sed '1,221d' diary-nofoot.txt > diary-noheadfoot.txt
~~~
{: .bash}

これは前と同じで、ヘッダー部分に対して行います。
綺麗なテキストになりました。
次のステップは、さらに厳密な分析の準備をします。
タイプして実行してください。

~~~
$ sed -e 's/<[^>]*>//g' diary-noheadfoot.txt > diary-notags.txt
~~~
{: .bash}

ここでは、正規表現を使い ([ライブラリ カーペントリーの正規表現レッスン] (https://librarycarpentry.org/lc-data-intro/01-regular-expressions/) を参照）、すべての有効な html タグ (角括弧内のもの) を見つけて削除します。
これは複雑な正規表現なので、どう動くかはあまり気にしないでください！ 
また、このスクリプトでは、これまで見てきた出力リダイレクト `>` と、まだ見ていない入力リダイレクト `<` の両方を使う必要があります。
まず、文字を解釈したり削除したりするのに使う `tr` コマンドを使います。
タイプして実行します。

~~~
$ tr -d '[:punct:]\r' < diary-notags.txt > diary-notagspunct.txt
~~~
{: .bash}

これはtrコマンドと特殊な文法を使って、句読点(`[:punct:]`)と改行(`\r`)を削除しています。
最後に、全ての大文字を小文字に変換し、テキストを正規化します。

~~~
$ tr [:upper:] [:lower:] < diary-notagspunct.txt > diary-clean.txt
~~~
{: .bash}

`diary-clean.txt`をテキストエディタで開いてください。
テキストがどのように変換されて、解析の準備ができるようになっているのか見てみましょう。

### テキストを分解し、単語出現頻度を数える
出力順に並べ替える。
テキストを分解する準備が整いました。

~~~
$ tr ' ' '\n' < diary-clean.txt | sort | uniq -c | sort -nr > diary-final.txt
~~~
{: .bash}

ここでは、[シェルを使用した集計とマイニング]({{ page.root }}{% link _episodes/05-counting-mining.md %})で見たパイプを拡張して使います。
このスクリプトの最初のところで、再度 translate コマンドを使い、すべての空白を `\n` に変換し、改行としてレンダリングします。
この段階で、ファイル内のすべての単語がそれぞれの行を持ちます。
次に、`sort`コマンドを使い、テキストを元の並びからアルファベット順に並べ替えます。
次に、もう一つの新しいコマンドである `uniq` の `-c` フラグを組み合わせ、重複した行を削除し、単語数を生成します。
最後の第４部は、3つ目のステップで生成された重複の数で再びテキストをソートする部分です。
これで、テキストを分解し、その中の単語を一つ一つ数えることができました。
これは、突合し可視化できるデータで、調査の基礎となる様式となるものであり、同じ方法で処理された別のテキストと比較することができます
別の分析のために別の変換セットを実行する必要がある場合は、`diary-final.txt`に戻り、その作業を開始することができます。
これらすべては、地味ではあるが非常に強力なコマンドライン上のいくつかのコマンドを使います。


## さらに詳しく

Deborah S. Ray and Eric J. Ray, *Unix and Linux: visual quickstart guide*, 4th edition (2009).
リファレンスガイドとして貴重（そして高額でない） - 特に、コマンドラインを散発的にしか使わない場合!

[The Command Line Crash Course](https://learncodethehardway.org/unix/)
'Learn UNIX the Hard Way' -- 基本を思い出すのに適している。

[Automate the Boring Stuff](https://automatetheboringstuff.com/)

Another Coursera course, [Programming for Everybody (Python)](https://www.coursera.org/course/pythonlearn)
is available and lasts 10 weeks, if you have 2-4 hours to spare per week.
もう一つのCourseraのコース、 [Programming for Everybody (Python)](https://www.coursera.org/course/pythonlearn)が利用でき、週2～4時間の時間があれば、10週間で完了します。

Pythonは可読性が高く、比較的シンプルで、非常に強力であるので、研究用プログラミングで人気があります。
ビル・ターケルとデジタル・ヒストリーのコミュニティは、より広い範囲に及んでいます
今日の2つ目のレッスンは、Billが[彼のウェブサイト](https://williamjturkel.net/2013/06/15/basic-text-analysis-with-command-line-tools-in-linux/)で公開しているレッスンを元にしたもので、Billは[Programming Historian](https://programminghistorian.org/project-team)の総編集者です。

Programming Historianは、歴史家にプログラミングのレッスンを提供することを目的とした、オープンかつ協同作業で作られた図書です。
歴史学者が抱える問題にフックしたレッスンですが、さまざまなプログラミング言語をカバーする彼らのレッスンは、幅広い応用が可能です。実際、今日の講座は、カナダのウォータールーに住む歴史学者、イアン・ミリガン氏と私がProgramming Historian用に書いた2つのレッスンがベースになっています。
Billはまた、['Named Entity Recognition with Command Line Tools in Linux'](http://williamjturkel.net/2013/06/30/named-entity-recognition-with-command-line-tools-in-linux/) という素晴らしいチュートリアルも持っていて、テキストファイル中の名前、場所、組織を自動的に検索、マークアップ、カウントしたい人にはぜひお勧めします...

## 結論
このセッションは、Unixシェルの操作、基本的なファイルのカウント、結合、削除、データ全体の共通文字列の問い合わせ、結果や派生データの保存、厳密な計算分析のためのテキストデータの準備を学びました。
ここでは、Unix環境でできることの表面に触れたに過ぎません。
このセッションが、さらなる調査や生産的な活動を促すのに十分なきっかけとなることを願っています。
ツールは、実際のプロジェクトに組み込んでこそ、その真価を発揮するものであることに留意ください。
何千ものファイルを操作し、数え、マイニングできることは、非常に便利です。
画像ファイルなど、英数字を含まない大量のファイルでも、ファイル名に含まれるメタデータの記述量に応じて、簡単にソート、選択、照会することができます。
Unix を使用するための前提条件ではないにせよ、時間をかけて一貫性のある予測可能な方法でデータを構造化することは、Unix コマンドを最大限に活用するための重要なステップであることは間違いないでしょう。
定期的にUnixシェルを使っていれば、例えばファイルをコピーしたり修正したりする程度であれば、基本を新鮮に保つことができ、次にもっと複雑なコマンドでUnixシェルを使う必要が生じたときに、もう一度学ぶ必要はないでしょう。



## References

James Baker and Ian Milligan, 'Counting and mining research data with Unix', *The Programming Historian* ([2014](https://programminghistorian.org/lessons/research-data-with-unix))

Ian Milligan and James Baker, 'Introduction to the Bash Command Line', *The Programming Historian* ([2014](https://programminghistorian.org/lessons/intro-to-bash))

William J. Turkel, 'Named Entity Recognition with Command Line Tools in Linux' ([30 June 2013](http://williamjturkel.net/2013/06/30/named-entity-recognition-with-command-line-tools-in-linux/)). The section 'NER Demo' is adapted from this and shared under a [Creative Commons Attribution-NonCommercial-ShareAlike 3.0 Unported](https://creativecommons.org/licenses/by-nc-sa/3.0/).

William J. Turkel, 'Basic Text Analysis with Command Line Tools in Linux' ([15 June 2013](https://williamjturkel.net/2013/06/15/basic-text-analysis-with-command-line-tools-in-linux/)). The sections 'Grabbing a text, cleaning it up' and 'Pulling a text apart, counting word frequencies' are adapted from this and shared under a [Creative Commons Attribution-NonCommercial-ShareAlike 3.0 Unported](https://creativecommons.org/licenses/by-nc-sa/3.0/).
