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
Lineコメントは規範がある。blockコメントはほとんどパッケージのコメントとして利用されるが、式内やコードの大規模な表示を向こうにするのに便利である。 
  
プログラムとWebサーバーのgodocはGoソースを読み込んで、パッケージの内容に関するドキュメントを抽出する。改行がない最も上にかかれたコメントは、宣言とともに説明として抽出される。それらのコメントの自然さとスタイルはgodocが生成するドキュメントのクオリティを決定づける。  
  
全てのパッケージは `package comment` (package節前のblockコメント)を持つべきである。複数のファイルのパッケージの場合、 `package comment` は一つのファイルにのみあればよく、どんなものでも同様である。`package comment` はパッケージの紹介をすべきで、パッケージ全体に関連した情報を提供する。  
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
  
コメントは`banners of stars`のような余分なフォーマットは必要ない。生成される出力は固定幅フォント上では表せられないかもしれない、そのためgofmtのようにalignmentのためのスペースに依存せず、それを気にしない。コメントはplain textを解釈しないため、HTMLや`_this_`のようなアノテーションは無視されるので、利用すべきでない。
godocがするような調整は、プログラムスニペットに適した固定幅フォント上でインデントされたtextを表示する。`fmt package`のパッケージコメントはこれを良い効果として利用している。  
  
コンテキストによると、godocはコメントをリフォーマットしないことがあるので、正しいスペルや句読点、文章構造、改行等々を適切に利用できているか注意したほうが良い。  
  
パッケージ内では、トップレベルの直前のコメントはパッケージの宣言のためのdoc commentとして機能する。すべてのプログラムの公開された大文字の名称は、doc commentを持つべきである。  
  
`Doc comments` は様々な automated presentationsが可能な完全な文章であることが望ましい。はじめの文章は宣言された名称で始まるまとまった一文であるべきである。  
```
// Compile parses a regular expression and returns, if successful, a Regexp
// object that can be used to match against text.
func Compile(str string) (regexp *Regexp, err error) {
```
  
もしま衣装が常にコメントの最初ならば、godocの出力に`grep`をかける際に役に立つ。あなたが"Compile"という名前を忘れてしまったが、正規表現をパースする関数を探したい場合を想像すると、下記のコマンドを実行することができる。
```
$ godoc regexp | grep parse
```
  
もしパッケージ内のすべてのdoc commentが "This function..." で始まった場合、`grep`は名前を思い出すのに役に立たないだろう。しかし、パッケージのコメントが名称ではじまることによって、探したい言葉を思い出させるような何かを見つけうるだろう。
```
$ godoc regexp | grep parse
    Compile parses a regular expression and returns, if successful, a Regexp
    parsed. It simplifies safe initialization of global variables holding
    cannot be parsed. It simplifies safe initialization of global variables
$
```
  
Goの宣言方法は文法上、グループにまとめての宣言が可能となっている。一つのdoc commentが関連した定数や変数のグループを紹介することができる。全体的な宣言となるため、そのようなコメントは形式的になりがちである。
```
// Error codes returned by failures to parse an expression.
var (
    ErrInternal      = errors.New("regexp: internal error")
    ErrUnmatchedLpar = errors.New("regexp: unmatched '('")
    ErrUnmatchedRpar = errors.New("regexp: unmatched ')'")
    ...
)
```
  
グルーピングはアイテム間の関係性を示しうる。(変数のセットがmutexによってプロテクトされているような例)
```
var (
    countLock   sync.Mutex
    inputCount  uint32
    outputCount uint32
    errorCount  uint32
)
```