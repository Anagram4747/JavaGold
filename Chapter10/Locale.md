# java.util.Locale
ロケール情報を扱うためのクラス<br>
地域や言語を表すために使用される<br>
「i18n(=internationalization)」を行うためのもの

## インスタンスの生成
以下の5つの方法のうち、どれかを用いてインスタンスを生成する
- getDefaultメソッド<br>
参考：[デフォルトのロケール情報の取得](#デフォルトのロケール情報の取得)
- コンストラクタ<br>
参考：[主な言語コード(ISO639-1)](#主な言語コード(ISO639-1))
- ロケール定数<br>
参考：[ロケール定数](#ロケール定数)
- ビルダー<br>
参考：[ビルダー](#ビルダー)
- ファクトリメソッド<br>
IETF言語タグを読み込むforLanguageTagメソッドを使用する

### デフォルトのロケール情報の取得
```java
import java.util.Locale;

public class Sample {
    public static void main(String[] args) {
        // デフォルトのロケール情報を取得する
        Locale locale = Locale.getDefault();
    }
}
```

### コンストラクタを用いたインスタンス生成
コンストラクタの引数として、**言語コード**や**国コード**などを受け取る

#### 主な言語コード(ISO639-1)
|言語|コード|
|----------|----------|
|日本語|ja|
|英語|en|
|ドイツ語|de|
|中国語|zh|
|フランス語|fr|

#### 主な国コード(ISO3166-1)
|国|コード|
|----------|----------|
|日本|JP|
|アメリカ合衆国|US|
|ドイツ|DE|
|中華人民共和国|CN|
|フランス|FR|

コンストラクタは、上記の言語コード、国コードの他、派生情報を受け取れるようオーバーロードされている
- Locale(String language)
- Locale(String language, String country)
- Locale(String language, String country, String variant)

### ロケール定数
Java SE 11では24種類のロケール定数が用意されている
|定数|言語|国|説明|
|----------|----------|----------|----------|
|static Locale.JAPAN|ja|JP|日本のロケール情報|
|static Locale.US|en|US|アメリカのロケール情報|
|static Locale.CANADA|en|CA|カナダのロケール情報|
|static Locale.CANADA_FRENCH|fr|CA|カナダのロケール情報|
|statix Locale.UK|en|GB|イギリスのロケール情報|

### ビルダー
Localeクラスのstaticインナークラスである、Builderクラスを使用する
```java
import java.util.Locale;

public class Sample {
    public static void main(String[] args) {
        // ビルダークラスを利用し、インスタンスを生成
        Locale locale = new Locale.Builder()
            .setLanguage("jp")
            .setRegion("JP")
            .setScript("Japan")
            .build();
    }
}
```
