# プロパティファイル
キーと値をペアにして保管するファイル<br>
ソフトウェアの国際化に対応する際、言語ごとに切り替えを行うためなどに使用される

## プロパティファイルの読み込み
java.util.Propertiesクラスを使用する

### 読み込み手順
1. プロパティファイルを読み込むjava.io.FileReaderオブジェクトのインスタンスを作成する
2. loadメソッドの引数に手順1で作成したインスタンスへの参照を渡す
3. PropertiesオブジェクトのgetメソッドやgetPropertyメソッドにキーを渡して値を取得する

```java
import java.io.FileReader;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.util.Properties;
import java.util.Set;

public class Sample {
    public static void main(String[] args) throws Exception {
        // インスタンスを生成して、プロパティファイルを読み込む
        Properties prop = new Properties();
        prop.load(new FileReader("settings.properties"));

        // キーの取得
        Set keys = prop.keySet();
        for (Object key: keys) {
            System.out.println(prop.get(key));
        }
    }
}
```

### メソッド
- get(Object key)<br>
値を取得する
- getProperty(String key)<br>
値を取得する
- list<br>
出力を引数に受け取り、プロパティの一覧を出力するためのメソッド

## プロパティファイルの変換
プロパティファイルは、ISO8859-1とUTF-8(Java SE 9以降)のみを扱うことができる<br>
Javaの標準ツールである、native2asciiを用い。日本語からUnicode表記へと変換する<br>
```bash
native2ascii inputFile.txt outputFile.properties
```

### UTF-8で書かれたプロパティファイルの読み込み
```java
import java.io.FileReader;
import java.nio.Charset.Charset;
import java.util.Properties;

public class Sample {
    public static void main(String[] args) throws Exception {
        Properties prop = new Properties();
        prop.load(new FileReader(
            "sample.properties",
            Charset.forName("UTF-8"));
        System.out.println(prop.getProperties("test"));
    }
}
```

## ResourceBundle
java.util.ResourceBundleという抽象クラスを用いることで、Localeを扱うことができる<br>
また、デフォルトでUTF-8にも対応している

### インスタンスの作成
抽象クラスであるため、staticメソッドを使用してインスタンスを作成する
```java
import java.util.ResourceBundle;

public class Sample {
    public static void main(String[] args) {
        // インスタンスの生成
        ResourceBundle resource = ResourceBundle.getBundle("sample");
    }
}
```
ResourceBundleクラスでロケーション情報を扱う場合、プロパティファイルの名前を設定することで、自動で読み込むファイルを切り替えることが可能<br>
[文字列]\_[言語]_[国名].propertiesというファイル名にした場合、デフォルトのロケール情報に対応するファイルが自動的に読み込まれるようになる<br>
ロケール情報に対応するプロパティファイルが存在しない場合、java.util.MissingResourceExceptionがスローされる

### メソッド
- getObject(String key)<br>
指定されたキーのオブジェクトを取得する
- getString(String key)<br>
指定されたキーの文字列を取得する
