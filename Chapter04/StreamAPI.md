# ストリームAPI
Java SE 8以降に実装された<br>
コレクションをより扱いやすくするための拡張

## 行うことができること
コレクションに対して処理を行う際に使用する<br>
具体的な用途としては以下の通りである
- コレクションや配列の全要素を同じように変換する場合
- コレクションや配列の要素の合計や平均といった統計を取る場合
- コレクションや配列の要素を何らかの条件でグルーピングする場合
- コレクションや配列の要素から条件に合ったデータを検索する場合


## ストリーム・パイプライン
ストリームに対して行う処理は、中間操作と終端操作と呼ばれる2つに分類される<br>
それぞれの操作はjava.util.stream.Streamインターフェースに定義されている
- 挙動<br>
ストリームの要素が順番にストリームパイプラインに渡され、中間操作→終端操作という順で処理が行われる

### 中間操作
ある処理を行い、ストリームを返すもの
|処理を行うメソッド|引数|概要|
|----------|----------|----------|
|distinct|なし|要素の重複を除いたストリームを戻す|
|filter|Predicate型|引数に指定した条件に一致する要素だけで構成されるストリームを戻す|
|limit|long|ストリームの要素を指定された数に切り詰めた結果のストリームを戻す|
|map|Function型|指定された関数を要素にお適用した結果のストリームを戻す|
|peek|Consumer型|新しい結果のストリームを作成し、指定された関数を適用して戻す|
|skip|long|引数で指定した最初のn個の要素を取り除き、残った要素で構成されるストリームを戻す|
|sorted|なし|ストリームの要素を自然順序にしたがってソートしたストリームを戻す|
|sorted|Comparator型|ストリームの要素を自然順序にしたがってソートしたストリームを戻す<br>自然順序の判定には、引数で指定されたcomparatorを使用する|

### 終端操作
ストリームを返さない処理<br>
作成された1つのストリームに対して、駆らなず1度しか行うことができない
|処理を行うメソッド|概要|
|----------|----------|
|allMatch|ストリーム内のすべての要素が条件に一致するかどうかを調べる|
|anyMatch|ストリーム内のいずれかの要素が条件に一致するかどうかを調べる|
|collect|ストリームの要素を持つコレクションを戻す|
|count|ストリーム内の要素の数を戻す|
|findAny|ストリーム内に要素が残っているかどうかの結果を持つOptionalを戻す<br>要素がある場合、ストリームの中のランダムな要素が格納されたOptionalが戻される|
|findFirst|ストリーム内の最初の要素を持ったOptionalを戻す|
|forEach|ストリーム内の要素を使って繰り返し処理を実行する|
|max|ストリーム内の最大の要素を戻す|
|min|ストリーム内の最小の要素を戻す|
|noneMatch|ストリーム内の要素で条件に一致するものがないかどうかを調べる|
|reduce|ストリーム内の要素を累積的に統合していくリダクション処理を実行する<br>戻り値の型は基本的にはOptional\<T>だが、初期値を指定した場合その型と同じ型となる|
|toArray|ストリームの要素を含む配列を戻す|

## ストリームの作成

### 配列からストリームを作成する
java.util.Arraysクラスのstreamメソッドを使用することで、ストリームを作成することができる<br>
プリミティブ型を扱うストリームでは、独自のストリームが作成される
|メソッドの定義|概要|
|----------|----------|
|static \<T> Stream<T> stream(T[] array)|配列からストリームを作成する|
|static IntStream stream(int[] array)|配列からIntStream型のストリームを作成する|
|static LongStream stream(long[] array)|配列からLongStream型のストリームを作成する|
|static DoubleStream stream(double[] array)|配列からDoubleStream型のストリームを作成する|

### 並列ストリームを作成する
java.util.CollectionインターフェースのparallelStreamメソッドを使用する<br>
要素の処理順は保障されないため、forEachOrderedメソッドのようなコレクションの保持順に処理を実行するメソッドを適切に利用する必要がある

## java.util.stream.Collectorインターフェース
ストリームの処理を行う際の途中経過を保持するオブジェクトを作成するためのインターフェース<br>
- java.util.stream.Collector\<T, A, R>という3つの型のパラメータを受け取る
  - T : ストリーム内の要素の型
  - A : 処理途中の値を保持するためのオブジェクト
  - R : 最終的な結果の型

### 抽象メソッド
|メソッド|戻り値|概要|
|----------|----------|----------|
|supplier|Supplier型|処理途中の値を保持するためのオブジェクトを作成するメソッド|
|accumulator|BiFunction型|実行したい処理を記述したラムダ式を戻す|
|combiner|BinaryOperator型|並列処理をしている際に、個々に作られた処理途中の値を保持するためのオブジェクトを結合する|
|finisher|Function型|処理結果を戻すラムダ式を戻す|
|characteristics|Collector.Characteristics型|コレクターの特徴を表すEnumを返す|

- accumulator<br>
[IT] 塁算器

### Collector.Characteristicsの列挙子
|列挙子|概要|
|----------|----------|
|CONCURRENT|並行処理をすることを表す|
|IDENTITY_FINISH|finisherメソッドが省略可能であることを表す|
|UNORDERED|操作を行う上での順序の維持を保証しないことを表す|

特に指定するものがない場合は、下記のように記述する
```java
import java.util.EnumSet;
import java.util.Set;
import java.util.stream.Collector;

public class SampleCollector implements Collector<Hoge, Hoge, Hoge> {
    // 省略

    // 以下記述方法
    @Orverride
    public Set<Characteristics> characteristics() {
        // 空のEnumSetが作成される
        return EnumSet.nameOf(Collector.Characteristics);
    }
}


```
