# セキュアコーディング

### 整数オーバーフロー攻撃
対策として、以下の3つの方法がある
- 事前条件テスト
- アップキャスト<br>
short ⇒ intの用に、よりメモリが多い型に変換する
- BigIntegerクラスを使う

### 特定のjarファイルにセキュリティ上の特権を与える
```
grent codeBase "file:/Dir/Paths/File.jar" {
    permission java.io.FilePermission "/", "read";
};
```
