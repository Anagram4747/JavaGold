# java.sql.Statementインターフェース

## サブインターフェース
- java.sql.PreparedStatement<br>
SQL文を扱うためのインターフェース<br>
パラメータを受け取り、SQL文を実行することが可能
- java.sql.CallableStatement<br>
ストアド・プロシージャを実行するためのインターフェース

### 比較
||コンパイル|パラメータ|
|----------|----------|----------|
|java.sql.Statement|executeメソッド実行時|受け取り不可|
|java.sql.PreparedStatement|インスタンス生成時|受け取り可|

## java.sql.Statement
PreparedStatementやCallableStatementをまとめるためのスーパーインターフェース

### メソッド
- executeBatch()<br>
一度に複数のSQL文を実行するためのメソッド<br>
addBatchメソッドを使い、SQL文を登録しておき後から一斉に実行することが可能

## java.sql.PreparedStatement
SQL文を実行するためのインターフェース<br>
パラメータを受け取りSQLのWhere句の条件を変更することなどが可能

### インスタンスの生成
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.PreparedStatement;

public class Sample {
    public static void main(String[] args) {
        final String url = "jdbc:mysql://localhost/test";
        try (var con = DriverManager.getConnection(url)) {
            String sql = "SELECT * FROM sampleDB";
            
            // インスタンス生成
            try (PreparedStatement ps = con.prepareStatement(sql)) {
                // do something
            }

        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}
```

### パラメータの設定
setXXXメソッドを使用することで、パラメータを設定することが可能<br>
※指定の際、左から何番目の?に代入するかを指定する(配列の添え字とは異なる)<br>
パラメータの設定を忘れると、実行時に例外がスローされる

### メソッド
- boolean execute()<br>
SQLの実行を行うメソッド<br>
SQLの実行結果が、ResultSet型のオブジェクトかどうかをboolean型で返す
- int executeUpdate()<br>
データの挿入・更新・削除を行うためのメソッド<br>
データ更新を行った件数を戻り値で返す
    - int executeUpdate(String sql)<br>
    java.sql.Statementクラスから継承したメソッド<br>
    継承したためコンパイルエラーになることはないが、実行しようとすると例外がスローされる

## java.sql.CallableStatement
ストアド・プロシージャを実行するためのクラス

### インスタンスの作成
```java
import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Sample {
    public static void main(String[] args) {
        final String url = "jdbc:mysql://localhost/test";
        try (var con = DriverManager.getConnection(url)) {
            // ストアド・プロシージャの指定
            String proc = "CALL STORED_PROCEDURE(?)";
            // インスタンス生成
            try (CallableStatement cs = con.prepareCall(proc)) {
                // パラメータ設定
                cs.setInt(1, 8);
                // ストアド・プロシージャの実行
                cs.execute();
            }

        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}
```
