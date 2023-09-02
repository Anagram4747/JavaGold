# Consoleクラス
Java SE 6から導入されたクラス<br>
コンソールからの入力の受け取りを簡略化したもの

## メソッド

- char[] readPassword()<br>
コンソールからパスワードを受け付ける<br>
入力中宇の内容は、コンソールには表示されない

## コンソールクラスの利用例
サンプルプログラム
```Java
import java.io.Console;

public class GetPassword {
    public static void main(String[] args) {
    Console console = System.console();
    char[] password = console.readPassword();
    System.out.println(String.valueOf(password));
}
```

サンプルプログラムの実行例
```bash
> java GetPassword
                        // ←この行にパスワードを入力(表示はされない)
password                // ←前の行に入力した内容が表示される
```
