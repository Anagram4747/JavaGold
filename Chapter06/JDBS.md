# JDBC
Java DataBase Connectivity

## コネクションの確立
DBMSとの接続を管理するためのインターフェースである、java.sql.Connectionを使用し、接続を行う<br>
Connectionインスタンスへの参照は、java.sql.DriverManagerクラスのgetConnectionメソッドを使って取得する

### Java SE 6以降でのコネクション確立の様子
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class JDBCSample {
    public static void main(String[] args) {
        // 接続文字列
        final String url = "jdbc:mysql://localhost/test";
        try {
            // DBとの接続の確立
            Connection con = DriverManager.getConnection(url);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (con != null) {
                    // 接続を閉じる
                    con.close();
                }
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

### 接続文字列
JDBCを利用してデータベースに接続するためには、URLを指定する必要がある
- mysql<br>
jdbs:mysql://[サーバー名]:[ポート番号]/[データベース名]
- Derby<br>
jdbs:derby://[サーバーのIPアドレス]:[ポート番号]/[データベースの場所]

## java.sql.Statementインターフェース
SQL文やストアド・プロシージャを扱うためのインターフェース群

### 参考
- [Chapter5/Statement.md](Statement.md)
