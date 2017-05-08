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