# Optionalクラス
java SE 8で導入された<br>
メソッドの処理結果を扱うためのクラス

## メソッド

### ファクトリメソッド
Optionalクラスには公開されたコンストラクタがないため、インスタンス生成の際はファクトリメソッドを使用する必要がある
|メソッド|説明|
|----------|----------|
|public static \<T> Optional\<T> empty()|空のOptionalのインスタンスを生成し、参照を戻す|
|public static \<T> Optional\<T> of(T value)|null以外の値を持ったOptionalのインスタンスを生成し、参照を戻す|
|public static \<T> Optional\<T> ofNullable(T value)|値をもっているか、値がnullの場合は空のOptionalのインスタンスを生成し、参照を戻す|

### getメソッド
Optionalクラスのインスタンスから値を取り出すために使用する
|メソッド|取得する値|Optionalクラスが空の場合の処理|
|----------|----------|----------|
|public T get()|Optionalクラスの値|java.util.NoSuchElementExceptionがスローされる<br>(java.util.RuntimeExeptionのサブクラスである)|
|public T orElse(T other)|Optionalクラスの値|otherの値を返す|
|public T orElseGet(Supplier<? extends T> supplier)|Optionalクラスの値|引数のラムダ式を実行してその値を戻す|
|public T orElseThrow()|Optionalクラスの値|NoSuchElementExceptionをスローする|
|public \<x extends Throwable> T orElseThrow(<br>&nbsp;&nbsp;Supplier<? extends X> exceptionSupplier<br>) throws X extends Throwable|Optionalクラスの値|引数で渡した関数を実行し、例外をスローする|
|public \<U> Optional\<U> map(<br>&nbsp;&nbsp;Function<? super T, ? extends U> mapper<br>)|mapperを適用した結果|空のOptionalクラスのインスタンス|
|public \<U> Optional\<U> flatmap(<br>&nbsp;&nbsp;Function<? super T, ? extends Optional<? extends U>> mapper<br>)|mapperを適用した結果<br>(戻り値の型がOptional<>となる)|空のOptionalクラスのインスタンス|

### 処理のみを行うメソッド
Optionalクラスの値は取り出さず、処理のみを行う
|メソッド|処理|
|----------|----------|
|public void ifPresent(Consumer<? super T> action)|値が存在する場合、値に対して引数で渡した処理を実行する|
|public void ifPresentOrElse(<br>&nbsp;&nbsp;Consumer<? super T> action,<br>&nbsp;&nbsp;Runnable emptyAction<br>)|値が存在する場合、actionで指定した処理を行う<br>存在しない場合、emptyActionで指定した処理を行う|

### チェックメソッド
Optionalクラスの中に値があるかどうかを調べる
|メソッド|戻り値|
|----------|----------|
|public boolean isPresent()|値がある場合はtrue<br>それ以外の場合はfalse|
|public boolean isEmpty()|値が存在しない場合はtrue<br>それ以外の場合はfalse|
