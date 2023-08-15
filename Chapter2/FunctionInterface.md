# 関数型インターフェース
ラムダ式やメソッド参照を渡すことで利用することができる

## 主な関数型インターフェース
|パッケージ|インターフェース|戻り値型|メソッド|
|----------|----------|----------|----------|
|java.util.function|Supplier\<T>|T|get()|
|java.util.function|Consumer\<T>|void|accept(T)|
|java.util.function|BiConsumer<T, U>|void|accept(T, U)|
|java.util.function|Predicate\<T>|boolean|test(T)|
|java.util.function|BiPredicate<T, U>|boolean|test(T, U)|
|java.util.function|Function<T, R>|R|apply(T)|
|java.util.function|BiFunction<T, U, R>|R|apply(T, U)|
|java.util.function|UnaryOperator\<T>|T|apply(T)|
|java.util.function|BinaryOperator\<T>|T|apply(T, T)|
|java.lang|Runnable|void|run()|
|java.util.concurrent|Callable\<V>|V|call()|

### 各単語の日本語訳
- インターフェース名
  - supplier<br>
  [名] 供給者
  - consumer<br>
  [名] 消費者
  - predicate<br>
  [他動] \[+that](~だと)断定する
    - I predicate that a dog has a soul. / 私は犬に魂があると断言する。
- メソッド名
  - get<br>
  [動] 取得する
  - accept<br>
  [他動] 受け入れる
  - test<br>
  [自動] 検査する
  - apply<br>
  \[他動] (資産などを)用いる<br>
  [自動] 適合する、申し込む
- その他
  - compose
  [他動] 構成する

## java.util.function.Supplierインターフェース
Supplier(提供者)という名前のとおり、引数を受け取らずに値を戻す処理を規定するもの<br>
get()メソッドを持つ

## java.util.function.Consumerインターフェース
Consumer(消費者)という名前のとおり、戻り値は戻さずに引数を受け取ってその引数を使用した処理を実行するメソッドを規定する<br>
accept()メソッドを持つ

### 類似インターフェースとその違い
- java.util.function.Consumer<br>
引数を1つ受け取り、処理を実行する
- java.util.function.BiConsumer<br>
引数を2つ受け取り、処理を実行する

## java.util.function.Predicateインターフェース
Predicate(断定する)という名前のから分かるように、何らかの判断をする関数を定義する<br>
test()メソッドを持つ

### デフォルトで継承されるメソッド
or()メソッドとand()メソッドが用意されている

```java
import java.util.function.Predicate;

// 0以上かどうかの判断
Predicate<Integer> isNaturalNumber = (x) -> x >= 0;
// 偶数かどうかの判断
Predicate<Integer> isEvenNumber = (x) -> x % 2 == 0;

// 「0以上」 or 「偶数」
isNaturalNumber.or(isEvenNumber)

// 「0以上」 and 「偶数」
isNaturalNumber.and(isEvenNumber)
```

### 類似インターフェースとその違い
- java.util.function.Predicate<br>
引数を1つ受け取り、真偽値を返す
- java.util.function.BiPredicate<br>
引数を2つ受け取り、真偽値を返す

## java.util.function.Functionインターフェース
引数を受け取り、戻り値を返す関数を定義する<br>
apply()メソッドを持つ

### デフォルトで継承されるメソッド
andThen()メソッドと、composeメソッドが用意されている

```java
import java.util.function.Function;

// インクリメントする
Function<Integer, Integer> increment = (x) -> x + 1;
// 2倍する
Function<Integer, Integer> twice = (x) -> x * 2;

// 「インクリメント」して「2倍」
// result1 = 22
let result1 = increment.andThen(twice).apply(10);

// 「2倍」して「インクリメント」
// result2 = 21
let result2 = increment.compose(twice).apply(10);
```

### 類似インターフェースとその違い
- java.util.function.Function<br>
引数を1つ受け取り、戻り値を返す
  - java.util.function.UnaryOparator<br>
  引数を1つ受け取り、同じ型の戻り値を返す<br>
  java.util.function.Functionを継承している
- java.util.function.BiFunction<br>
引数を2つ受け取り、戻り値を返す
  - java.util.function.BinaryOparator<br>
  同じ型の引数を2つ受け取り、それと同じ型の戻り値を返す<br>
  java.util.function.BiFunctionを継承している

## java.lang.Runnableインターフェース
Threadクラスで記述する、runメソッドのみを持つインターフェース<br>

### 参考
- [Chapter3/Thread.md](../Chapter3/Thread.md)
- [Chapter3/Executor.md](../Chapter3/Executor.md)

## java.util.concurrnet.Callableインターフェース
Java SE 5で導入された<br>
Threadクラスに渡すための処理であるという点はRunnableインターフェースと同様だが、**戻り値を返すことができる, 例外をスローすることができる**という点が異なる

### 例外
Callableインターフェースから投げられる例外は、java.util.concurrent.ExecutionExeprion型(検査例外)である<br>
callメソッドの中で投げた例外は、ExecutionExeptionの内部に保持されているため、Throwableから引き継いだgetCauseメソッドを使用して取り出すことができる

### 参考
- [Chapter3/Executor.md](../Chapter3/Executor.md)
