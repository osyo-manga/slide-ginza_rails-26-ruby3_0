#### 銀座Rails#28@リンクアンドモチベーション
- - -

### 12月25日にリリースされる
### Ruby 3.0 に備えよう！

---

#### [前回の銀座Rails#26 のハイライト](https://ginza-rails.connpass.com/event/189892/)
- - -

* Ruby 3.0 で型周りのサポートや並列処理を行うライブラリが入る                    <!-- .element: class="fragment" -->
* private def value(value) = value => @value つらすぎワロタ                    <!-- .element: class="fragment" -->
* Ruby 2.7.2 から deprecated warning がデフォルトででなくなる                    <!-- .element: class="fragment" -->


---

#### 自己紹介
- - -

* 名前：osyo
* Twitter : [@pink_bangbi](https://twitter.com/pink_bangbi)
* github  : [osyo-manga](https://github.com/osyo-manga)
* ブログ  : [Secret Garden(Instrumental)](http://secret-garden.hatenablog.com)
* 趣味で Ruby にパッチを投げたり bugs.ruby で気になったチケットをブログにまとめたりしてる
    * [一人 bugs.ruby Advent Calendar 2020](https://qiita.com/advent-calendar/2020/bugs_ruby) やってます
    * [気になった bugs.ruby まとめ](https://secret-garden.hatenablog.com/archive/category/bugs.ruby)
* Ruby で一番好きな機能は Refinements
* 最近 AST から Ruby のコードを復元する記事や Ruby 基礎文法最速マスターって記事を書いたので気になる人は読んでみてね
    * [Ruby の AST から Ruby のソースコードを復元しよう](https://secret-garden.hatenablog.com/entry/2020/12/01/093316)
    * [令和時代の Ruby 基礎文法最速マスター](https://secret-garden.hatenablog.com/entry/2020/12/01/232816)
* [Ruby 3.0 Advent Calendar 2020](https://qiita.com/advent-calendar/2020/ruby3) やってます

---

#### 今日話すこと
- - -

* 前々回の[銀座Rails#26](https://ginza-rails.connpass.com/event/189892/)で Ruby 3.0 の話をした
    * その時は preview1 時点での話をした
* preview1 から大きく挙動が変わった機能や新機能などがあるので再度 Ruby 3.0 について話に来た
* 今回は先日リリースされた preview2 よりも新しい 12/17 時点での最新版の Ruby での話をする

---

## 注意
- - -

* このスライドでは基本的に 2020/12/17 時点での Ruby の開発版で動作確認を行っています
* 現在進行形でまだ開発は進んでおり実際に Ruby 3.0 がリリースされた時点で挙動が変わっている可能性があります
* 実際に使用する場合はリリースノート等の情報を参照してください
* また Ruby 3.0 ではいくつかの機能が実験的に入っています
    * その機能を使うと experimental warning が出力される
    * この機能は将来的に挙動が変わる可能性があるので注意して使用する

---

#### Ruby 3.0 の概要
- - -

* Ruby 3.0 は 2020/12/25 にリリースされます！！！
* Ruby 3.0 では以下のような機能が導入される予定
    * パターンマッチ（正式）
    * 型周りをサポートする機能
    * 右代入（一部実験的）
    * 非同期IO をサポートする機能
    * エンドレスメソッド定義（実験的）
    * 並行・並列処理を行うためのライブラリ（実験的）
* 一方で Ruby 3.0 ではいくつか非互換な変更が入っている
    * キーワード引数や include / prepend 周り
    * バグ修正された結果、挙動が変わっているものもある
    * バージョンを上げる際には注意する必要がある

---

# 新機能

---

#### Ruby 3.0 の型周りのサポート
- - -

* Ruby 3.0 では型周りをサポートするライブラリがいくつか入る
    * RBS: Ruby で型情報を定義する .rbs ファイルを扱うライブラリ
    * TypeProf: Ruby コードを解析して .rbs を自動生成するライブラリ
* Ruby 3.0 では標準では型チェックを行う機能は入らない
    * steep という外部ライブラリで .rbs を使った型チェックを行うことができる
* RBS と TypeProf に関しては前回話したので今回は省略
    * [前回のスライド](https://speakerdeck.com/osyo/korekarafalse-ruby-tojin-false-ruby-nituite)を参照してね
* [TypePorf デモ](https://mame.github.io/typeprof-playground/#rb=%23+test.rb%0Aclass+User%0A++def+initialize%28name%3A%2C+age%3A%29%0A++++%40name%2C+%40age+%3D+name%2C+age%0A++end%0A++%0A++attr_reader+%3Aname%2C+%3Aage%0Aend%0A%0Auser+%3D+User.new%28name%3A+%22John%22%2C+age%3A+20%29&rbs=)
* 参照：
    * [Ruby 3 の静的解析ツール TypeProf の使い方 - クックパッド開発者ブログ](https://techlife.cookpad.com/entry/2020/12/09/120314)
    * [Ruby 3の静的解析機能のRBS、TypeProf、Steep、Sorbetの関係についてのノート - クックパッド開発者ブログ](https://techlife.cookpad.com/entry/2020/12/09/120454)


---


#### Ractor (実験的)
- - -

* Ractor は Ruby で並列処理を行うためのライブラリ
    * Ruby + Actor の略
* Ruby 3.0 では実験的に導入される予定
* 基本的な使い方は前回話したときと変わってない
* preview1 と比較して共有可能オブジェクト周りのサポートが追加されている
    * Ractor.make_shareable や Ractor.shareable? の追加
    * Proc オブジェクトが共有可能になる
    * マジックコメント shareable_constant_value で定数が自動的に共有可能オブジェクト化される
    * 共有可能オブジェクトに関しては[こちら](https://techlife.cookpad.com/entry/2020/11/20/110047)に詳しく書かれている
* Ractor に関しては現在進行形で開発されているので細かいところが変わるやも…


>>>

* 以下のコードを実行すると hello と world が混ざって出力される

```ruby
# Ractor.new のブロックが並列処理として実行される
# .new の引数をブロックの引数として受け取る事ができる
ractor = Ractor.new(10) do |loop_count|
  loop_count.times do
    puts :hello
    sleep 0.3
  end
end

# 並列処理が終了するまでブロッキングする
# p ractor.take

10.times do
  puts :world
  sleep 0.3
end
```

### [デモ](https://wandbox.org/permlink/2ngrcocPPT3hUMTB)

>>>

* ネストしたオブジェクトを共有可能オブジェクトにする
    * 参照：[[Feature #17274] Ractor.make_shareable(obj)](https://bugs.ruby-lang.org/issues/17274)

```ruby
hash = { a: "a", b: [1, 2, 3], c: { d: { e: [4, 5] } } }

# 共有可能オブジェクトかどうか判定する
pp Ractor.shareable?(hash)     # => false

# ネストしたオブジェクトを一括で共有可能オブジェクトにする
Ractor.make_shareable(hash)

pp Ractor.shareable?(hash)     # => true
# ネストして freeze されている
pp hash.frozen?                # => true
pp hash[:b].frozen?            # => true
pp hash[:c][:d].frozen?        # => true
```

>>>

* Ractor.make_shareable で Proc オブジェクトが共有可能になる
    * 参照：[[Feature #17284] Shareable Proc](https://bugs.ruby-lang.org/issues/17284)

```ruby
a = 1
block = proc { |x| a + x }
# 共有可能オブジェクトでない場合エラーになる
# error: allocator undefined for Proc (TypeError)
# Ractor.new (block) do |block|
#   block.call(Ractor.receive)
# end

# Proc を共有可能オブジェクトにする
Ractor.make_shareable(block)
pp Ractor.shareable?(block)   # => true

# 参照している値が変わってもブロックには反映されない
a = 3
pp block.call(4)   # => 5

# 共有可能オブジェクトだと OK
ractor = Ractor.new (block) do |block|
  block.call(Ractor.receive)
end
ractor.send(42)
p ractor.take     # => 43
```

>>>

* マジックコメントで自動的に定数を共有可能オブジェクト化する
    * [[Feature #17273] shareable_constant_value pragma](https://bugs.ruby-lang.org/issues/17273)
* 任意の行から差し込める

```ruby
# マジックコメント以下の定数が共有可能オブジェクト化
# shareable_constant_value: experimental_everything
A = [1, 2, 3]
pp Ractor.shareable?(A)    # => true

# shareable_constant_value を無効化する
# shareable_constant_value: none
B = "homu"
pp Ractor.shareable?(B)    # => false

# リテラルだったときのみ許可する
# shareable_constant_value: literal
# OK
C = "homu"

# error: unshareable expression
D = "homu" + "mami"
```

---

#### Scheduler
- - -

* IO のブロッキング処理を非同期で行うことを目的とした機能
    * 他の言語だと async/await のような機能に近い
    * Fiber を使って非同期処理を行う
* Ruby 3.0 では自分で Scheduler インターフェースを定義してそれを Fiber に設定して使用する
    * これにより Fiber で非同期 IO 処理を記述する事ができる
    * Ruby 3.0 ではこの概念が入った
* [デモ](https://wandbox.org/permlink/9vUzwFkuN9IuiITd)
* [http を読み込むサンプルコード](https://github.com/ruby/ruby/blob/8e03e3b0baf12b0e470ef7188559097fea95cb37/test/fiber/http.rb)
    * 試してみたけど動かなかった…
* 参照:
    * [[EN] Don't Wait For Me! Scalable Concurrency for Ruby 3! / Samuel Williams @ioquatix - YouTube](https://www.youtube.com/watch?v=Y29SSOS4UOc)
    * [doc/scheduler.md](https://github.com/ruby/ruby/blob/master/doc/scheduler.md)
    * [Scalable Web Applications - RubyWorld Conference 2020](https://youtu.be/OXQ9brlu26Q?t=6980)

>>>

```ruby
require "fiber"
require_relative 'scheduler'

$stdout.sync = true

puts "Go to sleep!"

# scheduler
# https://github.com/ruby/ruby/blob/8e03e3b0baf12b0e470ef7188559097fea95cb37/test/fiber/scheduler.rb
Fiber.set_scheduler(Scheduler.new)

Fiber.schedule do
  puts "Going to sleep"
  sleep(1)
  puts "I slept well"
end

puts "Wakey-wakey, sleepyhead"
```

---

#### パターンマッチ
- - -

* Ruby 2.7 から実験的に入った機能で Ruby 3.0 から正式に導入される
* 基本的には Ruby 2.7 から大きくは変わっていない
* Ruby 3.0 から find パターンがかけるようになった

```ruby
case ["a", 1, "b", "c", 2, "d", "e", "f", 3]
in [*pre, String => x, String => y, *post]
  p pre  #=> ["a", 1]
  p x    #=> "b"
  p y    #=> "c"
  p post #=> [2, "d", "e", "f", 3]
end
```

---

#### 1行 in (実験的)
- - -

* パターンマッチを 1行でかける構文
    * これはパターンマッチと同様に Ruby 2.7 で入った
* これは Ruby 3.0 では実験的な機能になります
* Ruby 3.0 では少しだけ挙動が変わり真理値を返すようになった

```ruby
case user
in { name: String, age: (..20) }
end
# 上のパターンマッチが 1行でかける
user in { name: String, age: (..20) }
```

```ruby
# マッチしなかった場合の挙動が変わった
{ name: "mami", age: 30 } in { name: String, age: (..20) }
# 2.7 => raise  NoMatchingPatternError
# 3.0 => false
```

>>>

* 真理値を返すようになったので条件式として利用できる

```ruby
user = { name: "mami", age: 30 }
# warning: One-line pattern matching is experimental, and the behavior may change in future versions of Ruby!
if user in { name: String, age: (..20) }
  puts "OK"
else
  puts "NG"
end
```

```ruby
users = [
  { name: "homu", age: 14 },
  { name: "mami", age: 15 },
  { name: "mado", age: 14 }
]
# warning: One-line pattern matching is experimental, and the behavior may change in future versions of Ruby!
pp users.select { _1 in { name: /^m/, age: (..15) } }
# => [{:name=>"mado", :age=>14}]
```

---

#### 右代入 (一部実験的)
- - -

* Ruby 3.0 では左辺の値を右辺の変数に代入する構文が入る
* これは preview1 から全く異なる機能になった
    * なので前回話した内容は全て忘れてください
* 最新版では真理値を返さない 1行 in と同じ挙動になった
* 最新版では真理値を返さない 1行 in と同じ挙動になった
    * つまり Ruby 2.7 のときの 1行 in とだいたい同じ挙動
* 経緯とか [[Feature #17371] Reintroduce `expr in pat` - Secret Garden(Instrumental)](https://secret-garden.hatenablog.com/entry/2020/12/17/010901)

>>>

* 基本的には 1行 in と同じだがマッチしなかったら例外が発生する

```ruby
# 左辺の値を右辺の変数に代入する
42 => value
```

```ruby
user = { name: "mami", age: 15 }
# パターンマッチのように特定のキーの要素を束縛できる
user => { name:, age: }
pp name  # => "mami"
pp age   # => 15

# もっと厳密にパターンを書くこともできる
user => { name: String => name, age: (..20) => age }
pp name
pp age
```

```ruby
# in とは違いパターンにマッチしなかったら例外が発生する
user => { name: Integer }
# => raise NoMatchingPatternError
```

>>>

* 単にローカル変数に代入するだけなら警告は出ないが、パターンマッチぽい記述をすると警告ができる

```ruby
# no warning
42 => value

# warning: One-line pattern matching is experimental, and the behavior may change in future versions of Ruby!
[42] => [value]
```

>>>

* ！！！注意！！！
* 右代入はローカル変数のみに代入でき、インスタンス変数やグローバル変数には代入できない
    * これはパターンマッチ自体がインスタンス変数やグローバル変数に値を束縛できない為
* あまり直感的ではないのでこれなんとかならないかな…

```ruby
# ローカル変数に代入しようとするとエラーになる
# syntax error, unexpected instance variable
42 => @value
# syntax error, unexpected global variable, expecting local variable or method
{ a: 42 } => { a: Integer => $value }

# パターンマッチでも同様にエラーになる
case 42
# syntax error, unexpected instance variable
in @value
end
case { a: 42 }
# syntax error, unexpected global variable, expecting local variable or method
in { a: Integer => $value }
end
```

>>>

* 1行 in と右代入の挙動まとめ

```ruby
# warning
42 in value
# no-warning
42 => value

# warning
[42] in [value]
# warning
[42] => [value]

[42] in [String]  # => false
[42] => [String]  # => raise NoMatchingPatternError

# error
42 in @value
# error
42 => @value
```


---

#### エンドレスメソッド定義
- - -

* Ruby 3.0 から 1行でメソッドが定義できるようになった
    * end を書かなくてメソッドが定義できるのでエンドレス
* preview1 から少しだけ挙動が変わった

```ruby
# end を書かずにメソッドが定義できる
def twice(a) = a + a

p twice 42   # => 84

# preview1 では () が必須だったが引数がない場合に限って現在は省略できる
def value = 42

# = 付きメソッドが定義できないのは現状もそのまま
# error: setter method cannot be defined in an endless method definition
def value=(value) = @value = value
```

>>>

#### 補足
- - -

* [開発者会議で1行 def について議論がされていた](https://github.com/ruby/dev-meeting-log/blob/master/DevelopersMeeting20201026Japan.md#feature-15921-r-assign-rightward-assignment-operator-aycabta)のでもしかしたら細かい挙動が変わるかも？

```ruby
def foo = expr    # change: allow if there is space between method name and "="; it shall be the same as "def foo; expr; end"
def foo() = expr => var # change: it shall be "def foo(); expr => var; end"
```

```ruby
# 現状はこうなっている
def foo() = expr => var
# は以下のように解釈される
(def foo() = expr) => var
```

---

#### 余談：private def value(value) = value => @value
- - -

* 前回 private def value(value) = value => @value がつらいという話をした
* preview1 時点では
    * private( { (def value(value) = value) => @value } )
* と解釈されていた
* 現在はどうなったのかというと…？

>>>

```ruby
# これは @value に右代入しようとしているのでエラーになる
# (def value(value) = value) => @value と同じ意味
# syntax error, unexpected instance variable
def value(value) = value => @value
```

```ruby
# これは右代入ではなくて Hash の要素として @value が参照される
# なのでエラー自体は preview1 と同じ
# private( { (def value(value) = value) => @value } )
# error: `private': {:value=>nil} is not a symbol nor a string (TypeError)
private def value(value) = value => @value
```

```ruby
# エンドレスメソッド定義の優先順位が変わるとエラーが変わる
# def foo() = expr => var が def foo(); expr => var; end になると
# private(def value(value) = (value => @value))
# になる
# ただし、 value => @value で結局エラーになる…
private def value(value) = value => @value
```

---

## 変更点やバグ修正

---

#### Module#include / prepend
- - -

* include / prepend 周りのバグがいくつか修正された
    * 参照：
        * [[Bug #7844] include/prepend satisfiable module dependencies are not satisfied](https://bugs.ruby-lang.org/issues/7844)
        * [[Bug #17038] On master, ancestry edits can lead to duplicates in Module#ancestors](https://bugs.ruby-lang.org/issues/17038)
        * [Bug #16852 Refining Enumerable fails with ruby 2.7](https://bugs.ruby-lang.org/issues/16852)
        * [[Bug #17130] Method#super_method is broken for aliased methods](https://bugs.ruby-lang.org/issues/17130)
        * [RubyWorld Conference 2020](https://youtu.be/OXQ9brlu26Q?t=3902) みてね！！
* これにより一部のコードの挙動が変わったりしているので注意する

>>>

* 最新版では『すでに include 済みのモジュールに対して include すると include 済みのオブジェクトにも継承リストが反映される』という挙動になってたりする

```ruby
module M
  def twice
    self + self
  end
end

p String.ancestors

Kernel.include M
# => [String, Comparable, Object, Kernel, BasicObject]

# 既存のクラスに M が反映されるようになる
p String.ancestors
# Ruby 2.7 => [String, Comparable, Object, Kernel, BasicObject]
# Ruby 3.0 => [String, Comparable, Object, Kernel, M, BasicObject]

# M のインスタンスメソッドが呼び出せるようになる
p "hoge".twice
# Ruby 2.7 => error: undefined method `twice' for "hoge":String (NoMethodError)
# Ruby 3.0 => "hogehoge"
```

>>>

* 以下のようにすると .ancestors に同じモジュールが複数含まれるようになる
* これにより今までの挙動と少し変わる可能性があるので注意する
    * 参照：[Ruby 3.0 で変わる Module#include の挙動 - Secret Garden(Instrumental)](https://secret-garden.hatenablog.com/entry/2020/09/04/163454)

```ruby
module M1; end

module M2; end

class X
  include M1
  include M2
end

M1.prepend M2

# Ruby 2.7 だとモジュールは重複しないが Ruby 3.0 だと重複するようになる
p X.ancestors
# 2.7 => [X, M2, M1, Object, Kernel, BasicObject]
# 3.0 => [X, M2, M2, M1, Object, Kernel, BasicObject]
```

>>>

* 更に次のような順番で prepend を行うと最後の prepend が反映されないようになるので注意する
* これが原因で Rails の一部が壊れた
    * [[Bug #16973] Rails Active Support unit test fails since 41582d5866](https://bugs.ruby-lang.org/issues/16973)
    * [[PR #39697] Use ActiveSupport::ToJsonWithActiveSupportEncoder#to_json for Ruby 2.8.0](https://github.com/rails/rails/pull/39697)

```ruby
module M
end

# Ruby 3.0 からはこれの prepend が Array にも反映されるようになる
Enumerable.prepend M
Array.prepend M

p Array.ancestors
# Ruby 2.7 => [M, Array, Enumerable, Object, Kernel, BasicObject]
# Ruby 3.0 => [Array, M, Enumerable, Object, Kernel, BasicObject]
```

---

#### Range リテラルと正規表現リテラルが frozen 化
- - -

* Ruby 3.0 では Range リテラルと正規表現リテラルがデフォルトで frozen 化される
* 参照：
    * [[Feature #15504] Freeze all Range object](https://bugs.ruby-lang.org/issues/15504)
    * [[Feature #16377] Regexp literals should be frozen](https://bugs.ruby-lang.org/issues/16377)

```ruby
pp (1..10).frozen?
# 2.7 => false
# 3.0 => false

pp /dog/.frozen?
# 2.7 => false
# 3.0 => false

range = (1..10)
# 2.7 : OK
# 3.0 : error: can't modify frozen #<Class:#<Range:0x0000556bacea7fc8>>: 1..10 (FrozenError)
range.instance_eval { @hoge = 42 }
```

---

#### Array や String のサブクラスのメソッドが変更
- - -

* Array や String のサブクラスのメソッドの戻り値が変更された
* 今までは一部のメソッドがサブクラスを返していたが Ruby 3.0 からは Array や String を返すようになった
* これの影響で Rails の一部が壊れた
    * [[PR #40663] Let AS::SafeBuffer#[] and * return value be an instance of SafeBuffer in Ruby 3.0](https://github.com/rails/rails/pull/40663)
* 参照：
    * [[Bug #6087] How should inherited methods deal with return values of their own subclass?](https://bugs.ruby-lang.org/issues/6087)
    * [[Bug #10845] Subclassing String](https://bugs.ruby-lang.org/issues/10845)
    * [【Ruby 3.0 Advent Calendar 2020】ArrayやStringのメソッドの返り値が変更された話【15日目】 - ゲームリンクスの徒然なる日常](https://gamelinks007.hatenablog.com/entry/2020/12/15/000000)

>>>

* Array#flatten や String#capitalize など一部のメソッドの戻り値が Array や String を返すようになった
* 具体的にどのメソッドが変更されたのかは [NEWS](https://github.com/ruby/ruby/blob/master/NEWS.md) を参照してください

```ruby
class MyArray < Array
end

p MyArray.new.flatten.class
# 2.7 => MyArray
# 3.0 => Array

class MyString < String
end

p MyString.new("hoge").capitalize.class
# 2.7 => MyString
# 3.0 => String
```

---

#### 非推奨な機能が削除される
- - -

* Ruby 3.0 では非推奨だったメソッドがいくつか削除される
    * ちなみに Ruby 2.7.1 では削除対象のメソッドを使用している場合はデフォルトで警告が出ているが 2.7.2 ではデフォルトで警告が出なくなっているので注意する
* もしかしたら 3.0 リリースまでに[他のメソッド](https://github.com/ruby/ruby/pull/3424)も削除されるかもしれません
* 参照：
    * [【Ruby 3.0 Advent Calendar 2020】Ruby3.0で非推奨から廃止になるメソッドたち【4日目】 - ゲームリンクスの徒然なる日常](https://gamelinks007.hatenablog.com/entry/2020/12/04/222254)

```ruby
# 2.7.1: warning: ENV.index is deprecated; use ENV.key instead
# 2.7.2: no warning
# 3.0.0: error: undefined method `index' for {...}
ENV.index("foo")
```

---

#### private の引数や attr_reader の戻り値が変更(かも)
- - -

* private public protected の引数の受け取り方と attr_reader attr_writer attr_accessor の戻り値が変わります
    * private などに配列を渡すとその配列の要素に対して適用されます
    * attr_reader などの戻り値は定義されたメソッド名のシンボルが配列として返ってきます
* 一応非互換になるのでちょっと注意
* で、今日この機能のマージされたんですがその後に [CI がランダムでコケているという報告があり](https://github.com/ruby/ruby/pull/3757#issuecomment-747868154)現在は Revert されています
    * https://github.com/ruby/ruby/commit/982443e6e373f5a3ac22ee495909cb9adffcd08d
    * Ruby 3.0 でどうなるのかはまだ未定…
* 参照：
    * [[Feature #17314] Provide a way to declare visibility of attributes defined by attr* methods in a single expression](https://bugs.ruby-lang.org/issues/17314)

>>>

```ruby
class Foo
  protected [:x, :y]
  # same as:
  protected :x, :y

  attr_accessor :foo, :bar # => [:foo, :foo=, :bar, :bar=] instead of `nil`
  attr_reader :foo, :bar # => [:foo, :bar] instead of `nil`
  attr_writer :foo, :bar # => [:foo=, :bar=] instead of `nil`

  alias_method :new_alias, :existing_method # => :new_alias instead of `Foo`
end
```

```ruby
# これを利用するとこんな感じで1行でかけるようになる
class Foo
  private attr_accessor :foo, :bar
end
```

---


#### その他
- - -

* `_1` という名前の変数が定義できなくなるよ
    * Numbered parameters と競合するので
* `$SAFE` や `$KCODE` がただのグローバル変数になる
    * [Rubyにはオブジェクトを汚染する仕組みがあった - いまブログ](https://imaizumimr.hatenablog.com/entry/2020/12/05/000000)
* [irb の複数行コードの貼付けがめっちゃ早くなった](https://github.com/ruby/reline/pull/186)
    * 2.7 と比較して 53倍早くなった
* `Hash#except` が標準に入った
* yaml のパフォーマンスが上がった
    * [[Bug #17101] YAML.load_file: Massive slowdown under Ruby 2.7 vs. Ruby 2.4](https://bugs.ruby-lang.org/issues/17101)
* [OpenStruct で既存のメソッドを呼び出せるようになった](https://secret-garden.hatenablog.com/entry/2020/12/08/000219)
    * `Object#then` みたいに定義済みメソッドと同名の要素が定義できるようになった

---

## まとめ

---

## まとめ
- - -

* 今回の内容は 2020/12/17 時点の話で Ruby 3.0 がリリースされるときには挙動が変わっている可能性もあるので注意してください
* 今回書ききれなかった機能とかはたくさんあるのでぜひ [NEWS](https://github.com/ruby/ruby/blob/master/NEWS.md) に目を通しておくといいと思います
* 個人的にはパターンマッチが一番注目の機能
    * 早くパターンマッチを使って気持ちよく Ruby のコードを書きたい…
* この話を聞いって Ruby 3.0 に興味を持たれた方はぜひぜひ preview2 や最新版の Ruby を試してもらえると幸いです
* Ruby 3.1 には新しい機能を追加したい

---

## 宣伝

---

#### 宣伝
- - -

* 毎月 Ruby Hacking Challenge in Hamada.rb というイベントが開催されています
* その名のとおり Ruby 本体をいじったりするようなイベントです
* Ruby の実装に興味がある方は参加してみるといいと思います
* [次回は 2021/01/19(火)](https://hamadarb.connpass.com/event/199016/) です


---

## 25日に Ruby 3.0 が無事にリリースされるの楽しみにしています！！！！

---


### ご清聴
### ありがとうございました

