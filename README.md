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

