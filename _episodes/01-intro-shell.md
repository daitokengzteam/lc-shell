---
title: "What is the shell?シェルって何？"
teaching: 5
exercises: 0
questions:
- "What is the shell?シェルって何？"
- "What is the command line?コマンドラインって何？"
- "Why should I use it?どうして使わないといけないの？"
objectives:
- "Describe the basics of the Unix shellシェルの基本を説明できるようになる。"
- "Explain why and how to use the command lineコマンドラインをなぜどのように使うのか説明できるようになる。"
keypoints:
- "The shell is powerfulシェルは強力です。"
- "The shell can be used to copy, move, and combine multiple filesシェルを使用して、複数のファイルをコピー、移動、結合することができます。"
---
## Introduction: What is the shell, and why should I use it?初めに：シェルって何ですか？何故、私がシェルを使うべきなの？

If you've ever had to deal with large amounts of data or large numbers of digital files scattered across your computer or a remote server, you know that copying, moving, renaming, counting, searching through, or otherwise processing those files manually can be immensely time-consuming and error-prone. Fortunately, there is an extraordinarily powerful and flexible tool designed for just that purpose.

あなたのコンピューターやリモートサーバーに散在する大量のデジタルファイルを自分のコンピュータとリモートにある大量のファイルを扱うには、ファイルのリネーム、集計、検索、またはその他の方法で処理すると、非常に時間がかかり、エラーが発生しやすくなります。幸いなことに、まさにその目的のために設計された非常に強力で柔軟なツールがあります。

The shell (sometimes referred to as the "Unix shell", for the operating system where it was first developed) is a program that allows you to interact with your computer using typed text commands. It is the primary interface used on Linux and Unix-based systems, such as macOS, and can be installed optionally on other operating systems such as Windows. 

シェル（最初に開発されたところのオペレーティングシステム（OS）ではUnixシェルと呼ばれることもあります）は、入力したテキストコマンドを使ってコンピューターと対話するためのプログラムです。それは、LinuxやUnixを元にしたシステムの主要なインターフェースで、macOSもそうですが、オプションでWindowsなどの他のOSにインストールできます。

It is the definitive example of a "command line interface", where instructions are given to the computer by typing in commands, and the computer responds by performing a task or generating an output. This output is often directed to the screen, but can be directed to a file, or even to other commands, creating powerful chains of actions with very little effort.

これは「コマンドラインインターフェース」のわかりやすい例で、コマンドを入力することでコンピュータに指示が与えられ、コンピュータはタスクを実行したり出力を生成することで応えます。出力は画面に表示されますが、出力をファイルに保存することもできます。他のコマンドに出力を受け渡すこともできます。他のコマンドに出力を渡すことで、一連のアクションを、わずかな労力で作成することができます。

Using a shell sometimes feels more like programming than like using a mouse. Commands are terse (often only a couple of characters long), their names are frequently cryptic, and their output is lines of text rather than something visual like a graph. On the other hand, with only a few keystrokes, the shell allows you to combine existing tools into powerful pipelines and to handle large volumes of data automatically. This automation not only makes you more productive, but also improves the reproducibility of your workflows by allowing you to save and then repeat them with a few simple commands. Understanding the basics of the shell provides a useful foundation for learning to program, since some of the concepts you will learn here—such as loops, values, and variables—will translate to programming.

シェルを使うというのは、マウスを使用するよりもプログラミングをするような感じです。コマンドは簡潔で（たいてい数文字程度）、出力はグラフのような視覚的なものではなく、何行かのテキストです。一方、シェルを使用すると、数文字のキーの入力で既存のツールを組み合わせて、強力なパイプラインを作り上げることができますし、大量のデータを自動的に処理することができます。この自動化はあなたの生産性を向上するだけではなく、単純なコマンドの組み合わせを保存したり、繰り返したりすることでワークフローの再現性をあげることができます。シェルの基本を理解することはプログラミングを学ぶ上での基礎になります。ここで学ぶいくつかのコンセプト、ループ（繰り返し処理）、値、変数は一部プログラミングに通じます。

The shell is one of the most productive programming environments ever created. Once mastered, you can use it to experiment with different commands interactively, then use what you have learned to automate your work. 

シェルは今までに作られた中でもっとも生産性が高いプログラミング環境の一つです。一旦マスターするとさまざまなコマンドを対話的に操作して、あなたの仕事を自動化するためにそれらを使うことができるようになります。

In this session we will introduce task automation by looking at how data can be manipulated, counted, and mined using the shell. The session will cover a small number of basic commands, which will constitute building blocks upon which more complex commands can be constructed to fit your data or project. Even if you do not do your own programming or your work currently does not involve the command line, knowing some basics about the shell can be useful.

このセッションでは少数の基本的なコマンドについて説明します。これらのコマンドはあなたのデータやプロジェクトに合わせて、より複雑なコマンドを作るときの組み立てブロックとなります
もし今の仕事がプログラミングに関係なくコマンドラインを使っていなくても、シェルに関するいくつかの基本を知っておくと役立ちます。

*Note to Lesson Instructor: Consider providing an example here of how you’ve used the Unix shell to solve a problem in the last week or month*

講師の皆様へ
ごく最近Unixシェルをどのように使っているか例を挙げて説明しましょう。

### Where is my shell?　私のシェルはどこですか？

The shell is a program that is usually launched on your computer much in the way you would start any other program. However, there are numerous kinds of shells with different names, and they may or may not be already installed. The shell is central to Linux-based computers, and macOS machines ship with Terminal, a shell program. For Windows users, popular shells such as Cygwin or Git Bash provide a Unix-like interface, but may need to be installed separately. In Windows 10, the PowerShell natively provides that functionality.

シェルというのは、あなたがプログラムを起動するときに通常起動するプログラムです。ただし、さまざまな名前のさまざまな種類のシェルがあり、すでにインストールされているかもしれませんし、されていないかもしれません。シェルは Linux ベースのコンピュータの中心です。macOSにはターミナルが付属し、Windows ユーザーの場合、ポピュラーなCygwin や Git Bash などのUnixに似たシェルのインターフェースを提供しますが、個別にインストールする必要がある場合もあります。 Windows 10では、PowerShell にてその機能を最初から使えます。

For this lesson, we will use Git Bash for Windows users, Terminal for macOS, and the shell for Linux users.

このレッスンでは、WindowsユーザーはGit Bush、macOSユーザーはターミナル、Linuxユーザーはシェルを利用します。
