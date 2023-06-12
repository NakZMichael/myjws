## リポジトリ名と違う名前でクローンしたいとき

```
git clone <リポジトリのURL> [ディレクトリ名]
```

## 簡単にファイルの状態を確認したいとき

```
❯ git status -s                                  
AM README.md
 M app/src/main/java/myjws/App.java
?? test.txt
```

## .gitignoreの基本

- 空行あるいは # で始まる行は無視される
- 標準の glob パターンを使用可能
- 再帰を避けるためには、パターンの最初にスラッシュ (/) をつける
- ディレクトリを指定するには、パターンの最後にスラッシュ (/) をつける
- パターンを逆転させるには、最初に感嘆符 (!) をつける

### globパターンの基礎

glob パターンとは、シェルで用いる簡易正規表現のようなものです。 アスタリスク (`*`) は、ゼロ個以上の文字にマッチします。 `[abc]` は、角括弧内の任意の文字 (この場合は a、b あるいは c) にマッチします。 疑問符 (`?`) は一文字にマッチします。 また、ハイフン区切りの文字を角括弧で囲んだ形式 (`[0-9]`) は、 ふたつの文字の間の任意の文字 (この場合は 0 から 9 までの間の文字) にマッチします。 アスタリクスを2つ続けて、ネストされたディレクトリにマッチさせることもできます。 `a/**/z` のように書けば、`a/z`、`a/b/z`、`a/b/c/z`などにマッチします。

### .gitignoreの例

```
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```

## addとcommitをまとめてやりたいとき

追跡対象となっているファイルを自動的にステージしてからコミットを行います。

```
git commit -a
```

## ファイルをGitの管理対象から外したいけど削除はしたくないとき


```
git rm --cached <ファイル名のglobパターン>

example:
git rm --cached log/\*.log
```

`*` の前にバックスラッシュ (`\`) があることに注意しましょう。 これが必要なのは、シェルによるファイル名の展開だけでなく Git が自前でファイル名の展開を行うからです。 このコマンドは、`log/` ディレクトリにある拡張子 `.log` のファイルをすべて削除します

## `git log`の出力を直近2件だけに絞りたいとき

```
git log -2
```

## `git log`で直近2週間だけみたいとき

```
git log --since=2.weeks
```

## `git log`で絞りたい時のオプション


- `-(n)`: 直近の n 件のコミットのみを表示する
- `--since`, `--after`: 指定した日付より後に作成されたコミットのみに制限する
- `--until`, `--before`: 指定した日付より前に作成されたコミットのみに制限する
- `--author`: エントリが指定した文字列にマッチするコミットのみを表示する
- `--committer`: エントリが指定した文字列にマッチするコミットのみを表示する
- `--grep`: 指定した文字列がコミットメッセージに含まれているコミットのみを表示する
- `-S`: 指定した文字列をコードに追加・削除したコミットのみを表示する
- `--`:  ディレクトリ名あるいはファイル名を指定すると、それを変更したコミットのみが対象となります。

## `git commit --amend`

ステージングされている内容を直近のコミットに追加したり、コミットメッセージを修正する。
```
`git commit --amend`
```

## ステージングエリアからファイルを移動させる

```
 git reset HEAD <ファイル名>
```

`git reset` は、危険なコマンドに_なりえます_。その条件は、「`--hard`オプションをつけて実行すること」です。
ただし、上述の例はそうしておらず、作業ディレクトリにあるファイルに変更は加えられていません。
`git reset` をオプションなしで実行するのは危険ではありません。 ステージングエリアのファイルに変更が加えられるだけなのです。

## ステージングされていない変更内容を元に戻す方法

```
git checkout -- <ファイル名>
もしくは
git restore <ファイル名>
```

ここで理解しておくべきなのが、`git checkout [file]`は危険なコマンドだ、ということです。 あなたがファイルに加えた変更はすべて消えてしまいます。変更した内容を、別のファイルで上書きしたのと同じことになります。そのファイルが不要であることが確実にわかっているとき以外は、このコマンドを使わないようにしましょう。

やりたいことが、「ファイルに加えた変更はとっておきつつ、一時的に横に追いやっておきたい」ということであれば、`stash`やブランチを調べてみましょう。一般にこちらのほうがおすすめの方法です。

Git にコミットした内容のすべては、ほぼ常に取り消しが可能であることを覚えておきましょう。 削除したブランチへのコミットや `--amend` コミットで上書きされた元のコミットでさえも復旧することができます (データの復元方法については データリカバリ を参照ください)。 しかし、まだコミットしていない内容を失ってしまうと、それは二度と取り戻せません。

## `origin`

クローン元のサーバーに対して Git がデフォルトでつける名前です。

`git remote-v`でリモートリポジトリのURLを確認できる。

originのブランチには`origin/<ブランチ名>`としてアクセスできる。

## リモートリポジトリの追加

```
git remote add <shortname> <url> 
```

## `git fetch`の意味

リモートプロジェクトのすべてのデータの中からまだあなたが持っていないものを引き出します。 実行後は、リモートにあるすべてのブランチを参照できるようになり、いつでもそれをマージしたり中身を調べたりすることが可能となります。

`git fetch` コマンドはデータをローカルリポジトリに引き出すだけだということです。 ローカルの環境にマージされたり作業中の内容を書き換えたりすることはありません。 したがって、必要に応じて自分でマージをする必要があります。

リモートブランチを追跡するためのブランチを作成すれば 、`git pull` コマンドを使うことができます。れは、自動的にフェッチを行い、リモートブランチの内容を現在のブランチにマージします。

た、 `git clone` コマンドはローカルの master ブランチ（実際のところ、デフォルトブランチであれば名前はなんでもかまいません）がリモートの master ブランチを追跡するよう、デフォルトで自動設定します。 `git pull` を実行すると、通常は最初にクローンしたサーバーからデータを取得し、現在作業中のコードへのマージを試みます。

## リモートブランチの内容を色々みたいとき

```
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/NakZMichael/myjws.git
  Push  URL: https://github.com/NakZMichael/myjws.git
  HEAD branch: main
  Remote branches:
    develop               tracked
    feature/from-external tracked
    main                  tracked
  Local branches configured for 'git pull':
    develop               merges with remote develop
    feature/from-external merges with remote feature/from-external
    main                  merges with remote main
  Local refs configured for 'git push':
    develop               pushes to develop               (up to date)
    feature/from-external pushes to feature/from-external (up to date)
    main                  pushes to main                  (up to date)

```


## タグの作成

Git のタグには、軽量 (lightweight) 版と注釈付き (annotated) 版の二通りがあります。
Git では、注釈付きのタグをシンプルな方法で作成できます。もっとも簡単な方法は、`tag` コマンドの実行時に `-a` を指定することです。

軽量版のタグは、変更のないブランチのようなものです。特定のコミットに対する単なるポインタでしかありません。

しかし注釈付きのタグは、Git データベース内に完全なオブジェクトとして格納されます。 チェックサムが付き、タグを作成した人の名前・メールアドレス・作成日時・タグ付け時のメッセージなども含まれます。 また、署名をつけて GNU Privacy Guard (GPG) で検証することもできます。 一般的には、これらの情報を含められる注釈付きのタグを使うことをおすすめします。 しかし、一時的に使うだけのタグである場合や何らかの理由で情報を含めたくない場合は、 軽量版のタグも使用可能です。

```
実行例
git tag -a v1.4 -m "my version 1.4"
```

`-m` で、タグ付け時のメッセージを指定します。これはタグとともに格納されます。注釈付きタグの作成時にメッセージを省略すると、エディタが立ち上がるのでそこでメッセージを記入します。

### 特定のコミットにタグをつける

```
実行例
git tag -a v1.2 9fceb02
```

### タグの共有

デフォルトでは、`git push` コマンドはタグ情報をリモートに送りません。 タグを作ったら、タグをリモートサーバーにプッシュするよう明示する必要があります。

その方法は、リモートブランチを共有するときと似ています。`git push origin [tagname] `を実行するのです。

多くのタグを一度にプッシュしたい場合は、 `git push` コマンドのオプション `--tags` を使用します。 これは、手元にあるタグのうちまだリモートサーバーに存在しないものをすべて転送します。

## タグの確認

```
git tag
v1.4
```

タグのデータとそれに関連づけられたコミットを見るには `git show` コマンドを使用します。

```
git show v1.4
tag v1.4
Tagger: Muka Nakazato <muka.nakazato@gmail.com>
Date:   Sat Jun 10 19:55:39 2023 +0900

my version 1.4
```

## タグへのチェックアウト

チェックアウトと同時に新しいブランチを作成する

```
git checkout -b [branchname] [tagname]
```

## エイリアスの設定

```
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
$ git config --global alias.graph 'log --graph'

```

Git が単に新しいコマンドをエイリアスで置き換えていることがわかります。 しかし、時には Git のサブコマンドではなく外部コマンドを実行したくなることもあるでしょう。 そんな場合は、コマンドの先頭に `!` をつけます。 これは、Git リポジトリ上で動作する自作のツールを書くときに便利です。 例として、git visual で gitk が起動するようにしてみましょう。

`$ git config --global alias.visual '!gitk'`
