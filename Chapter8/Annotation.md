# アノテーション
Java SE 5.0にて導入された<br>
マーカーインターフェースを簡単に実現することを目的としたもの

### マーカーインターフェース
インターフェースを何も持たず、クラスに意味を追加することだけを目的としたインターフェース<br>
Java.io.Serializableが代表的なマーカーインターフェースに当たる

## マーカーインターフェースと異なる点
- プロセッサに値を渡すことができる
- フィールドやメソッド、コンストラクタなど細かな単位でマークすることができる

## アノテーションの定義と利用
```java
// アノテーションの宣言
public @interface sampleAnnotation {
    // アノテーションで値を扱う
    String strValue();
    int intValus();
}

// アノテーションの利用
@sampleAnnotation(strValue = "name", intValue = 100)
public class UseAnnotation {
    // 処理
}
```

## メタアノテーション
アノテーションを定義するためのアノテーション

### @Retention
アノテーションの保持期間を定義するアノテーション<br>
java.lang.annotationパッケージのRetentionPolicy列挙子を使用する
|種類|内容|
|----------|----------|
|CLASS|アノテーション情報が、クラスファイルには残るが実行時に破棄される|
|RUNTIME|実行時までアノテーション情報を保持する|
|SOURCE|コンパイル時にアノテーション情報を破棄する|

### @Target
アノテーションの対象を指定するアノテーション<br>
java.lang.annotation.ElementType列挙子を使用する
|列挙子|対象|
|----------|----------|
|ANNOTATION_TYPE|アノテーション宣言|
|CONSTRUCTOR|コンストラクタ宣言|
|FIELD|フィールド宣言|
|LOCAL_VARIABLE|ローカル変数宣言|
|METHOD|メソッド宣言|
|MODULE|モジュール宣言|
|PACKAGE|パッケージ宣言|
|PARAMETER|メソッドの引数宣言|
|TYPE|クラス、インターフェース、Enum宣言|
|TYPE_PARAMETER|型パラメータ宣言|
|TYPE_USE|型の使用|

## 代表的なアノテーション

### @SuppressWarnings
コンパイラの警告を無視するアノテーション
- unchecked<br>
すべてのコンパイラの警告を抑制する
- deprecation<br>
非推奨のものを使用してるときの警告を消すためのアノテーション
- removal<br>
削除予定の強く非推奨とされているものを使用しているときの警告を消すためのアノテーション
