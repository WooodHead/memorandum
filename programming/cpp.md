# C++

## 設計

### template
template meta programmingでのtemplateの分類
下記分類に当てはまらないようなtemplateの使い方は避ける

1. traitsによる型に対する関数
2. duck typingによる型の振る舞いの指定


* 単体テストは、仕様として想定している型についてすべてかく
* 単体テストは、ユーザにとっての振る舞いを中心にテストする
    * テストの厳密性は捨てる
    * 仕様として保証する型についての動作に集中する
    * すべての型についてcv, reference, pointerをテストするのが望ましいが無理な場合は型として受け入れることを想定しているものについてはテストする。
* 型が境界値を持つように設計する


### privateな関数
publicなinterfaceにのみテストが必要であると思うと、 privateな関数にテストが必要な時は設計を一旦見直す必要がある。
class内のprivateなメソッドをprivateでない形にするには以下の3通りが考えられる。
状況として、publicな`TargetClass.publicMethod()`メソッド内で`TargetClass.privateMethod()`メソッドなるprivateメソッドを使っている状況を考える。

1. ある名前空間に属す関数とする
    * `namespace::privateMehod()`を宣言して、`publicMethod()`内で使用する。
2. クラスのstaticメソッドとする
    * 静的なクラスを作成し、そのクラスの`static StaticClass.privateMethod()`メンバ関数とする
3. そのメソッド（と関係するメソッドがあれば）を持つクラス（オブジェクト）を新たに作る
    * `PrivateMethodClass.privateMethod()`を作り、`PrivateMethodClass`クラス内ではPublicな`privateMethod`をテストする。
    `TargetClass`のコンストラクタで、`PrivateMethodClass`を渡sて、`TargetClass.publicMethod()`内で使用する。
4. inteface用のクラスを作成`virutal InterfacePrivateMethodClass.privateMethod() = 0`、その具象クラスとして`PrivateMethodClss.priavteMethod()`をテストする。
    * 3と同様だが、4はinterface用のクラスを作り動的ポリモーフィズムに備える。

#### 1
* 良い
    * 他のクラスでも使用できる
    * `TargetClass`のコンストラクタの引数が増えない
* 悪い
    * 何でもかんでも関数化すると関数であふれる
    * 適切なファイル配置が難しい場合がある

#### 2
* 良い
    * 他のクラスでも使用できる
    * `TargetClass`のコンストラクタの引数が増えない
    * interfaceの違いに影響されない
* 悪い
    * 意味ある
    * 適切なクラス名の付与が難しい場合がある

#### 3
* 良い
    * 
* 悪い
    * コンストラクタで渡す必要がある 

#### 4
* 良い
    * 
* 悪い
    * コンストラクタでクラスを渡す必要がある 
    * interfaceとして利用できないクラスには使えない
        * 例えば、具象クラスとして`TargetClass.privateMethod()`の実装しかないなど。


## 環境変数

### g++
下記の環境変数のパスのheader fileを探す。

```shell
CPATH
C_INCLUDE_PATH
CPLUS_INCLUDE_PATH
OBJC_INCLUDE_PATH
```

下記の環境変数のパスのlibファイルを探す。

```shell
LD_LIBRARY_PATH
```

### clang
下記の環境変数のパスのheader fileを探す。

```shell
CPATH
C_INCLUDE_PATH
CPLUS_INCLUDE_PATH
OBJC_INCLUDE_PATH
```

```shell
LIBRARY_PATH
```

## pyclewn
debugger

http://pyclewn.sourceforge.net/

## template

### templateクラスの型推論
```cpp
template <typename E>
class test {

};

template <typename E>
class base {

};

class derived : public base <derived> {

};

template <typename E>
struct traits {
    static void apply() {
        std::cout << "traits<E>" << std::endl;
    }
};

template <typename E>
struct traits<test<E> > {
    static void apply() {
        std::cout << "traits<test<E> >" << std::endl;
    }
};

template <typename E>
struct traits<base<E> > {
    static void apply() {
        std::cout << "traits<base<E> >" << std::endl;
    }
};

int main(int argc, char const* argv[])
{
    traits<double>::apply();
    traits<test<double> >::apply();
    traits<test<base<test<double> > > >::apply();
    traits<base<derived> >::apply();
    traits<derived>::apply();
    traits<test< base<derived> > >::apply();
    traits<base<test<double> > >::apply();
    /*
    output here
    traits<E>
    traits<test<E> >
    traits<test<E> >
    traits<base<E> >
    traits<E>
    traits<test<E> >
    traits<base<E> >
    */
    return 0;
}
```

型推論時に型の継承関係は考慮されない。

```cpp
template <typename E>
class base {

};

class derived : public base <derived> {

};

template <typename E>
struct traits {
    static void apply() {
        std::cout << "traits<E>" << std::endl;
    }
};

template <typename E>
struct traits<base<E> > {
    static void apply() {
        std::cout << "traits<base<E> >" << std::endl;
    }
};

int main(int argc, char const* argv[])
{
    traits<derived>::apply();
    /*
    output here
    traits<E>
    */
    return 0;
}

```


## typedef

### 配列のtypedef

```cpp
//int[2]のtypedef
typedef int type[2];
```

### typedefのprivate, public

```cpp
class A {
private:
    typedef int private_type;
public:
    typedef int public_type;
};

void main() {
    //A::private_type hoge; //error
    A::public_type hoge;
}
```

### typedefの継承
```cpp
class base {
public:
    typedef int type1[1];
    typedef int type2[1];
public:
    static int apply1()
    {
        return sizeof(type1);
    }
    static int apply2()
    {
        return sizeof(type2);
    }
};

class derived {
public:
    typedef int type2[2];
public:
    static int apply1()
    {
        return sizeof(type1);
    }
    static int apply2()
    {
        return sizeof(type2);
    }
};

void main()
{
    std::cout << base::apply1() << std::endl; //4
    std::cout << derived::apply1() << std::endl; //8
    std::cout << base::apply2() << std::endl; //4
    std::cout << derived::apply2() << std::endl; //4
}

```

## Autimatic differentiation
実装の方法は

1. operator overloadingで、演算結果の変わりに演算のオブジェクトを返す
    * 演算結果からObjectを取り出す時に、値とderivativeの評価をする
    * 式のオブジェクトをクラスとして持つ為、explicit instanciationができない
2. operator overloading時に、値と微分値の計算結果を返す
    * 一時オブジェクトを作るか、計算結果を左オペランドにいれるか、何かしらのstorageが必要
    * 式を持たないので、explicit instanciationが使える。

### 1について
`dual_add`などを作る

### 2について


## tips

### case1: CRTPのインスタンス化の順序
CRTPでtypedefするときの注意。
下記はderivedのtypedefされる前にbaseのtypedefが評価されるが、このときderivedのtypedefは評価されていないのでエラー。
```cpp
template <typename T>
struct base {
    typedef typename T::value_type value_type;
    static const T& operator()()
    {
        return static_cast<const T&>(*this);
    }
};

struct derived : base<derived> {
    typedef double value_type;
};
```


## c++コンパイラの仕組み

1. プリプロセッサが`main.cpp`を解析し、includeやマクロを展開する。
```shell
cpp -E main.cpp -o main.ii
```
2. コンパイラが展開されたソースファイルを解析し、アセンブリコードに変換
```shell
cc1plus -S main.cpp -o main.s
```
3. アセンブラがアセンブリコードからオブジェクトファイルを生成
```shell
as -c main.s -o main.o
```
4. リンカがオブジェクトファイルをまとめて実行ファイルを生成
    * `ld`はオブジェクトファイルとアーカイブ`.a`を静的にリンクする。
```shell
ld main.o 
```


### オブジェクトファイルの中身を見る
objdumpコマンドを使う。
* `-r`
    * リロケーション情報
* `-R`
    * 動的リロケーション情報
* `-x`
    * オブジェクトファイル内のヘッダーの全情報
* `-t`
    * オブジェクトファイル内のヘッダーのSYMBOL情報
* `-T`
    * オブジェクトファイル内のヘッダーのDynamicSYMBOL情報

## pre_compiled_header

### gcc
下記で`pre_compiled_header.h`から`pre_compiled_header.h.pch`が生成される。

```shell
g++ -I/path/to/hearder pre_compiled_header.h
```

`pre_compiled_header.H`の中身は下記の通り。

```cpp
#pragma once
#include <boost/shared_ptr.hpp>
```

### clang
下記で`pre_compiled_header.h`から`pre_compiled_header.h.pch`が生成される。

```shell
clang++ -cc1 -emit-pch -o pre_compiled_header.h.gch pre_compiled_header.h
```

```shell
clang++ -include-pch pre_compiled_header.h.gch source.cpp
```

## clang

### c++11
下記を指定

```shell
clang++ -std=c++11 -stdlib=libc++ main.cpp
```

### mac
macでのclangのupdateはxcodeからupdateする。

[ref](http://minus9d.hatenablog.com/entries/2013/08/06)

### error
* `-fcxx-exceptions`

```shell
error: C++ exceptions was disabled in PCH file but is currently enabled
```

* `-fexceptions`

```shell
error: exception handling was disabled in PCH file but is currently enabled
```

* `-x c++`
* `-x c++-headers`
* `-fblocks`

```shell
error: blocks extension to C was disabled in PCH file but is currently enable
```

* `-fmax-type-align=16`
    * 数字は？

```
error: default maximum alignment for types differs in PCH file vs. current file
```


## Tips

### size_t型 is always unsigned
`size_t`型は常にunsignedと規格できまっている。
但し、32bit環境と64bit環境ではサイズが異なる可能性がある。

* C++ standard section 18.1で`size_t`は`<cstddef>`に、C Standardの`<stddef.h>`と同じものとして定義される。
* C Standard Section 4.1.5で、`size_t`は非負整数という記述がある。

* [c++ - is size_t always unsigned? - Stack Overflow](http://stackoverflow.com/questions/1089176/is-size-t-always-unsigned)

## 開発環境
* [C/C++プログラマのための開発ツール](http://www.slideshare.net/herumi/cc-66035712)

## 参考
* [isocpp](https://isocpp.org/)
* [msgpack/msgpack-c: MessagePack implementation for C and C++ / msgpack.org](https://github.com/msgpack/msgpack-c)
* [とあるエンジニアの備忘log: インライン関数まとめ](http://masahir0y.blogspot.com/2012/08/blog-post.html)
* [Extensible Templates: Via Inheritance or Traits?](http://www.gotw.ca/publications/mxc++-item-4.htm)

* [コンセプト・チェックの利用 - boostjp](http://boostjp.github.io/archive/boost_docs/libs/concept_check/using_concept_check.html)
* [一人でBoost勉強会:Concept Check #1 - Qiita](http://qiita.com/termoshtt/items/47c8e3a7b2b84b3c883c)
* [一人でBoost勉強会:Concept Check #2 - Qiita](http://qiita.com/termoshtt/items/8e91c50dbf299376442f)
* [一人でBoost勉強会:Concept Check #3 - Qiita](http://qiita.com/termoshtt/items/fd343dbff0d12c94d243#_reference-c4ab26d42a6adfde8f09)
* [コンセプトは滅びぬ！何度でもよみがえるさ！コンセプトの力こそC++erの夢だからだ！ - spinorの日記](http://d.hatena.ne.jp/spinor/20111215/1323951052)
* [C++1y 軽量Concept - Faith and Brave - C++で遊ぼう](http://faithandbrave.hateblo.jp/entry/20130226/1361862112)
* [C++17の標準ライブラリ](http://ezoeryou.github.io/cpp17lib-slide/#/)
* [EzoeRyou/cpp-history: Presentation slide for C++ history](https://github.com/EzoeRyou/cpp-history)
* [EzoeRyou/boost-benkyo-18: Boost勉強会#18大阪の発表の資料](https://github.com/EzoeRyou/boost-benkyo-18)

* [C++のムーブと完全転送を知る](http://proc-cpuinfo.fixstars.com/2016/03/c.html/)

