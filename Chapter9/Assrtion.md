# アサーション
事前・不変・事後条件判定を行う機能<br>
デフォルトでは無効となっているため、javaコマンド実行時に、-eaオプションをつけることで使用可能

## 構文
assert 条件式 : メッセージ;

```java

public void setPrice(int price) {
    assert pricce < 0 : "invalid price : " + price;
}
```
