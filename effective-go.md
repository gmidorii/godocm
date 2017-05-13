# Effective Go

## Introduction
Goは新しい言語です。すでにある言語からアイディアを借りているけれど、
他の言語にない特徴によりGoプログラミングを効率的にする珍しいpropertiesを持っている。
C++やJavaを直接Goに変換すると微妙になる場合がある。(JavaプログラミングはGoでなくJavaで書くものです)一方で、Goの視点からプログラムを考えることで成功することができるが、それはかなり違ったプログラムになるだろう。  
言い換えると、Goをよく書くためには、propertiesとidiomを理解することが重要である。また、自分の書いたコードが他のGopherが理解しやすようにするために、よくある監修について知ることも大切である。(naming, formatting, プログラム構成, etc..)  
  
このドキュメントは、明確で慣用的なGoコードのためのtipsを紹介します。  
ですが、下記の3つをまず最初に読んで欲しい
- [language specification](http://golang-jp.org/ref/spe)
- [A Tour of Go](https://tour.golang.org/welcome/1)
- [How To Write Go Code](http://golang-jp.org/doc/code.html)

### Examples
[Go package sources](http://golang-jp.org/src/) はcoreライブラリだけでなく、言語のhowtoの例もサーブしようとしている。  
また、多くのパッケージには [golang.org](https://golang.org/) で動かせる自己完結した実行例を含んでいる。([このように](https://golang.org/pkg/strings/#example_Map))  
もし問題のアプローチ方法や実装方法について疑問がある場合、ライブラリ内のドキュメントのコードや例が答え、idea、背景を与えることができるだろう。


## Formatting
フォーマッティングの問題は、最も争われる問題だがもっとも重要でないことだ。
人々は異なるフォーマットスタイルに適応しうり、皆が同じスタイルを使うことでこのフォーマットの話題に割く時間を削減できる。  
問題はスタイルガイド無しでUtopiaにたどり着く方法である。  
  
Goでは普通でないアプローチをとっており、それは機械がすべてのフォーマッティングを気にさせるようにしている。  
gofmtプログラム(`go fmt` として利用でき、それはソースレベルでなくパッケージレベルの操作をします)はGoプログラムを読み込み、標準スタイルでインデントや垂直方向の揃い方、必要であればコメントもリフォーマットしたものを出力します。  
もし新しいレイアウトを揃える方法を知りたい場合、 `gofmt` 実行します。もし結果が正しくないようなら、プログラムを組み換えれて(もしくは `gofmt` のバグです)、それを回避はしません。
  
例として、structrureのフィールドコメントを並び替える必要はありません。
`Gofmt` にておこなえます。宣言が下記のように与えられている場合
```go
type T struct {
    name string // name of the object
    value int // its value
}
```
`gofmt` で列をそろえると
```go
type T struct {
    name    string // name of the object
    value   int    // its value
}
```
すべての標準パッケージのGoコードは `gofmt` でフォーマットされている。  
いくつかのフォーマットの詳細を簡単に示すと、
- Indentation
  - インデントはtabで行い、`gofmt`のデフォルトになっている。スペースは使わなければいけないときのみ利用する。
- Line length
  - 行の長さのリミットはない。punched card オーバーフローについて心配する必要はない。もし行が長すぎる場合、extra tabでインデントしてラップできる
- Parentheses(括弧)
  - GoではCやJava肉あらべて括弧の必要性は少ない。制御構造(if, for, switch) はsyntax上括弧がいらなく、また演算子の優先順位はより短くよりクリアのため、他の言語と違ってスペースが意味を持ちます
  ```
  x<<8 + y<<16
  ```

## Commentary
GoはCスタイルの /* */blockコメントとC++スタイルの//lineコメントが提供されている。  
Lineコメントは規範がある。blockコメントはほとんどパッケージのコメントとして利用されるが、式内やコードの大規模な表示を向こうにするのに便利です。  
  
プログラムとWebサーバーのgodocはGoソースを読み込んで、パッケージの内容に関するドキュメントを抽出します。改行がない最も上にかかれたコメントは、宣言とともに説明として抽出される。それらのコメントの自然さとスタイルはgodocが生成するドキュメントのクオリティを決定づける。  
  
全てのパッケージは `package comment` (package節前のblockコメント)を持つべき。複数のファイルのパッケージの場合、 `package comment` は一つのファイルにのみあればよく、どんなものでも同様である。`package comment` はパッケージの紹介をすべきで、パッケージ全体に関連した情報を提供する。  
`package comment`はgodocページの最初に出てきて、以下に続く詳細ドキュメントのセットアップをすべきである。
```go
/*
Package regexp implements a simple library for regular expressions.

The syntax of the regular expressions accepted is:

    regexp:
        concatenation { '|' concatenation }
    concatenation:
        { closure }
    closure:
        term [ '*' | '+' | '?' ]
    term:
        '^'
        '$'
        '.'
        character
        '[' [ '^' ] character-ranges ']'
        '(' regexp ')'
*/
package regexp
```
  
もしパッケージがシンプルなら、パッケージコメントは簡潔にできうる。
```go
// Package path implements utility routines for
// manipulating slash-separated filename paths.
```
  
コメントは`banners of stars`のような余分なフォーマットは必要ない。生成される出力は固定幅フォント上では表せられないかもしれない、そのためgofmtのようにalignmentのためのスペースに依存せず、それを気にしない。