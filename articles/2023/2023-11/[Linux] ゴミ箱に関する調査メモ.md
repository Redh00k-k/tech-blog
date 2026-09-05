# 目的
ファイルやディレクトリをゴミ箱に移動させるプログラムを作成した時の調査メモを備忘録として記録する。

# ゴミ箱の場所
Freedesktop.orgの仕様[^1]によると、デスクトップ上にある「ゴミ箱」は`$XDG_DATA_HOME/Trash`にあるとのこと。なお、`$XDG_DATA_HOME`が設定されていない、或いは空の場合には`$HOME/.local/share`がデフォルトとして使用される[^2]。

Trash配下には以下のサブディレクトリがあり、これらを活用してゴミ箱機能を実現・管理している。
- `/info`
    - `/files` にあるファイルとディレクトリを管理するための情報ファイルが格納されるディレクトリ
    - 情報ファイルは`/files`にあるファイルらと同一名で拡張子に`.trashinfo`が付与されたファイルが格納されている(例: hoge.txtをゴミ箱に移動させたら、hoge.txt.trashinfo)
- `/files`
    - ゴミ箱に入れられたファイルとディレクトリが格納されるディレクトリ


# <ファイル・ディレクトリ名>.trashinfoの実装
Freedesktop.orgにサンプルが掲載されている。
```
[Trash Info]
Path=foo/bar/meow.bow-wow
DeletionDate=20040831T22:32:08
```

`.trashinfo`が準拠すべき仕様は以下の通り。
- 最初の行は必ず`[Trash Info]`でなければならない
- `Path=`と`DeletionDate=`以外のキーペアが存在する場合は他の行を無視する
- 複数の`Path=`や`DeletionDate=`がある場合は、最初に記載されているものが使用される

他にも付随する仕様に以下のものがある。
- ファイルやディレクトリをゴミ箱に入れるとき、実装は最初に`$trash/info`に対応するファイルを作成しなければならない
- `/info`にはサブディレクトリがないこと
- etc.

# ゴミ箱内に同名ファイル・ディレクトリが存在する場合
GUIなどを使ってファイル・ディレクトリをゴミ箱に移動させる際、ゴミ箱内に同一名のファイル・ディレクトリが存在している場合には名前に".数字"を追加して保存する模様。
```
$ ls ~/.local/share/Trash/info/
hoge.2.trashinfo   hoge.trashinfo   test.2.txt.trashinfo   test.txt.trashinfo

$ ls ~/.local/share/Trash/files/
hoge  hoge.2  test.2.txt  test.txt
```

`info`ディレクトリ内にある`Path=`には同一であることから、`Path=`以降の文字列を参照して「元に戻す」機能を実現している
```
$ cat ~/.local/share/Trash/info/test.txt.trashinfo  
[Trash Info]
Path=/tmp/test.txt
DeletionDate=2023-11-05T01:46:55Z

$ cat ~/.local/share/Trash/info/test.2.txt.trashinfo
[Trash Info]
Path=/tmp/test.txt
DeletionDate=2023-11-05T10:47:15
```

[^1]: https://specifications.freedesktop.org/trash-spec/trashspec-latest.html
[^2]: https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html
