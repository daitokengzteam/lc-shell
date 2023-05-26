---
title: "ループで面倒なことを自動化する"
teaching: 20
exercises: 10
questions:
- "ループとはなにか"
- "繰り返しの仕事をするのにどのようにループを使うか。”
objectives:
- "ループを使って複数のファイルを繰り返しリネームできるようになる。"
- "Implement a loop to rename several files"
keypoints:
- "ループは生産性向上の鍵"
- "なぜならこれは繰り返し実行することでワイルドカードやタブ補完とどうようにタイピングの量と間違いを避けることができる"
---

### Writing a Loop ループを書く

**Loops** are key to productivity improvements through automation as they allow us to execute
commands repetitively. Similar to wildcards and tab completion, using loops also reduces the
amount of typing (and typing mistakes).
Suppose we have several hundred document files named `project_1825.txt`, `project_1863.txt`, `XML_project.txt` and so on.
We would like to change these files, but also save a version of the original files, naming the copies
`backup_project_1825.txt` and so on.

**ループ** は自動化による作業効率の向上の鍵となるものです。ループを使うと、コマンドを何度も繰り返して実行することができます。
また、ワイルドカードやタブ補完と同様に、ループを使用すると、文字をタイプする数（やタイプの間違い）を減らすこともできます。
たとえば、 `project_1825.txt`, `project_1863.txt`, `XML_project.txt` といったファイルが何百個もあるとします。
これらのファイルを編集すると同時に、バックアップを `backup_project_1825.txt` のようなファイル名で保存しておくには、どうすればよいでしょうか。

We can use a **loop** to do that. 
Here's a simple example that creates a backup copy of four text files in turn.

そのようなことをするために **ループ** が使えます。
以下に、4つのテキストファイルのバックアップを順番に作成する、かんたんな例を示します。

Let's first create those files:

まず、対象のファイルを作ります:

~~~
$ touch a.txt b.txt c.txt d.txt
~~~
This will create four empty files with those names. It is easy to use the shell to create a batch of files in one go.

このコマンドで、指定した名前を持つ4つのからのファイルが作成されます。シェルでは複数のファイルを1行で作ることもかんたんに行えます。

Now we will use a loop to create a backup version of those files. First let’s look at the general form of a loop:

次に、これらのファイルのバックアップを作成するために、ループを使います。ループの一般的な構成を見てみましょう:

```
for thing in list_of_things
do　do
    operation_using $thing    # Indentation within the loop is not required, but aids legibility ここにdoとdoneの間に作業を書くインデントを書くと見やすくなります。
done
```
{: .language-bash}

We can apply this to our example like this:

これを私たちの例にあてはめると以下のようになります:

~~~
$ for filename in ?.txt
> do
>    echo "$filename"
>    cp "$filename" backup_"$filename"
> done
~~~
{: .bash}

~~~
a.txt
b.txt
c.txt
d.txt
~~~
{: .output}

When the shell sees the keyword `for`,
it knows to repeat a command (or group of commands) once for each thing `in` a list.
For each iteration,
the name of each thing is sequentially assigned to
the **loop variable** and the commands inside the loop are executed before moving on to
the next thing in the list.
Inside the loop,
we call for the variable's value by putting `$` in front of it.
The `$` tells the shell interpreter to treat
the **variable** as a variable name and substitute its value in its place,
rather than treat it as text or an external command.

シェルが `for` というキーワードを見つけると、
リスト `in` で指定されたリストの中身に対して、コマンド（もしくはその集合）をそれぞれ一度ずつ繰り返すようになります。
繰り返しの中で、リストの中身の値には **ループ変数** が割り当てられ、ループの中のコマンドが次の行に進む前に実行されます。
ループの中では、ループ変数の値は、先頭に `$` をつけて呼び出すことができます。
`$` は、シェルのインタープリターのプログラムに、 **変数** を文字列やコマンドとして扱うのではなく変数の名前として扱い、値をその変数に代入するように指示します。

> ## Double-quoting variable substitutions 変数の代入とダブルクォーテーション
>
> Because real-world filenames often contain white-spaces,
> we wrap `$filename` in double quotes (`"`). If we didn't, the
> shell would treat the white-space within a filename as a separator
> between two different filenames, which usually results in errors.
> Therefore, it's best and generally safer to use `"$..."` unless
> you are absolutely sure that no elements with white-space can ever
> enter your loop variable (such as in [episode 5]({{ page.root }}/05-counting-mining/index.html#using-a-loop-to-count-words)).
> 
> 実際のファイルネームにはスペースが含まれることがあるので、
> ここでは変数 `$filename` をダブルクォーテーションマーク (`"`) で囲っています。
> もしこれがないと、シェルはひとつのファイル名に含まれるスペースを、ふたつのファイル名の区切りとして扱ってしまい、エラーになるかもしれません。
> このため、どのファイル名にもスペースが含まれていないと確信していない限りは、 `"$..."` のように変数名をダブルクォーテーションマークで囲うのがベストで安全です。
{: .callout}

In this example, the list is four filenames: 'a.txt', 'b.txt', 'c.txt', and 'd.txt'
Each time the loop iterates, it will assign a file name to the variable `filename`
and run the `cp` command.
The first time through the loop,
`$filename` is `a.txt`.
The interpreter prints the filename to the screen and then runs the command `cp` on `a.txt`, (because we asked it to echo each filename as it works its way through the loop).
For the second iteration, `$filename` becomes
`b.txt`. This time, the shell prints the filename `b.txt` to the screen, then runs `cp` on `b.txt`. The loop performs the same operations for `c.txt` and then for `d.txt` and then, since
the list only included these four items, the shell exits the `for` loop at that point.

この例では、リストは4つのファイル名です: 'a.txt', 'b.txt', 'c.txt', and 'd.txt'
ループが繰り返されるたびに、シェルはファイル名を変数 `filename` に割り当て、 `cp` コマンドを実行します。
ループの最初の繰り返しでは、 `$filename` の値は `a.txt` です。
インタープリターはファイル名を画面に出力し、 `cp` コマンドを `a.txt` に対して実行します（これは、ループの実行中にファイル名を表示するように指定しているためです）。
2回めの繰り返しでは、 `$filename` は `b.txt` になります。このとき、シェルはファイル名 `b.txt` を画面に表示し、 `cp` コマンドを `b.txt` に対して実行します。同様に、 `c.txt` と `d.txt` にも実行します。リストにはファイルが4つしか含まれていないので、この時点でシェルは `for` ループを終了します。

> ## Follow the Prompt プロンプトを追う
>
> The shell prompt changes from `$` to `>` and back again as we were
> typing in our loop. The second prompt, `>`, is different to remind
> us that we haven't finished typing a complete command yet. A semicolon, `;`,
> can be used to separate two commands written on a single line.
> ループの中で文字を入力していると、シェルプロンプトはドルマークから大なり記号に変わり、またもとに戻ります。
> ふたつ目のプロンプトの大なり記号は、ループの入力を完了していないことを示します。
> 1行の中で2つ以上のコマンドを入力するには、セミコロン `;` が区切り文字として使えます。
{: .callout}

> ## Same Symbols, Different Meanings 同じシンボルでも異なる意味
>
> Here we see `>` being used as a shell prompt, but `>` can also be
> used to redirect output from a command (i.e. send it somewhere else, such as to a file, instead of displaying the output in the terminal) ---
> we'll use redirection in [episode 5]({{ page.root }}{% link _episodes/05-counting-mining.md %}).
> Similarly, `$` is used as a shell prompt, but, as we saw earlier,
> it is also used to ask the shell to get the value of a variable.
>
> If the *shell* prints `>` or `$` then it expects you to type something,
> and the symbol is a prompt.
>
> If *you* type `>` or `$` yourself, it is an instruction from you that
> the shell to redirect output or get the value of a variable.
> 同じ文字で違う意味
> プロンプトで大なり記号を使いましたが、大なり記号は出力をリダイレクトするのに使います。
> 例えば端末する代わりにファイルに出力
> これはリダイレクションといった5章で紹介します。
> ドルマークはシェルプロンプトでも使いますが変数を使うときにも使います。
> もしこのシェルの出力で大なり記号やドルマークがでてきたとき待っている状態です。
> この記号はプロンプトです。
> シェルの中で大なり記号を入力した場合、リダイレクトすることの指示です。
> 変数の中身を取得するという指令になります。意味が違います。
{: .callout}

We have called the variable in this loop `filename`
in order to make its purpose clearer to human readers.
The shell itself doesn't care what the variable is called.

私たちは、このループの中で変数の名前を `filename` としています。これは、人間が読んだときに変数の目的をわかりやすいようにするためです。
変数の名前は、シェルの動作には影響しません。

> ## For loop exercise
> Complete the blanks in the for loop below to print the name, first line, and last line
> of each text file in the current directory.
>練習問題穴埋めです。
> ```
> ___ file in *.txt
> __
> 	echo "_file"
> 	head -n 1 _______
> 	____ __ _ _______
> ____
> ```
> {: .bash}
>
> > ## Solution
> > ```
> > for file in *.txt
> > do
> > 	echo "$file"
> > 	head -n 1 "$file"
> > 	tail -n 1 "$file"
> > done
> > ```
> > {: .bash}
> {: .solution}
{: .challenge}

This is our first look at loops. We will run another loop in the
[Counting and Mining with the Shell]({{ page.root }}{% link _episodes/05-counting-mining.md %}) episode.

![For Loop in Action](../fig/shell_script_for_loop_flow_chart.svg)

> ## Running the loop from a Bash script
> ## Bashスクリプトでループを実行する
>
> Alternatively, rather than running the loop above on the command line, you can 
> save it in a script file and run it from the command line without having to rewrite
> the loop again. This is what is called a Bash script which is a plain text file that 
> contains a series of commands like the loop you created above. In the example script below, 
> the first line of the file contains what is called a Shebang (`#!`) followed by the path to the interpreter 
> (or program) that will run the rest of the lines in the file (`/bin/bash`). The second line demonstrates how 
> comments are made in scripts. This provides you with more information about what the script does. 
> The remaining lines contain the loop you created above. You can create this file in the same directory 
> you've been using for the lesson and by using the text editor of your choice (e.g. nano) but when you save the 
> file, make sure it has the extension **.sh** (e.g. `my_first_bash_script.sh`). When you've done this, you can run the
> Bash script by typing the command bash and the file name via the command line (e.g. `bash my_first_bash_script.sh`). 
> バッシュスクリプトといいます。コマンドを書いたプレーンテキストです。下のレーンのようにシェバンっていうのがあって
> 2番めの行はコメントです。
> 3番目からの行はプログラム保存することができます。
> ただしそのときに.shで作ってください。
> > ```
> > #!/bin/bash
> > # This script loops through .txt files, returns the file name, first line, and last line of the file
> > # このスクリプトはすべての .txt ファイルに対してループを実行し、ファイル名、ファイルの最初の行、ファイルの最後の行を出力します
> > for file in *.txt
> > do
> > 	echo $file
> > 	head -n 1 $file
> > 	tail -n 1 $file
> > done
> > ```
> > {: .bash}
> Download/copy [my_first_bash_script.sh](https://raw.githubusercontent.com/LibraryCarpentry/lc-shell/gh-pages/files/my_first_bash_script.sh). For more on Bash scripts, see [Bash Scripting Tutorial - Ryans Tutorials](https://ryanstutorials.net/bash-scripting-tutorial/).チュートリアルがありますので、もっとみてみてください。
{: .callout}
