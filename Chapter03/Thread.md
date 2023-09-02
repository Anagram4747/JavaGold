# スレッドによる並列処理

### 並行処理と並列処理の違い
- 並行処理<br>
シングルコアで、複数のタスクを高速に切り替えながら実行する
- 並列処理<br>
マルイチコアで、複数のタスクを同時に実行する

### 並列処理にかかわる英単語
- concurrent<br>
[形] 同時の<br>
[IT] 並行処理
- parallel
[形] 平行の
[IT] 並行処理

## 並列処理の実現方法
- java.lang.Threadクラスを継承したサブクラスを定義する
- java.lang.Runnableインターフェースを実現したクラスを用意し、そのインスタンスをThreadクラスのコンストラクタに渡す
  - Runnableインターフェースは、Threadクラスに記述するrunメソッドのみを持つインターフェース

### 実際の使用方法
1. Threadクラスを継承するクラスを作成
2. 1で作成したクラスのrunメソッドを定義
3. 1, 2で定義したクラスのインスタンスを作成
4. 3で作成したインスタンスに対して、startメソッドを実行する

## java.lang.Threadクラス

### メソッド
- run()<br>
新しいスレッドで実現したい処理を記述する
- start()<br>
Threadクラスのインスタンスを生成し、startメソッドを実行することで、新しいスレッドでrunメソッドの中身が実行される
- sleep(long millis)<br>
現在実行中のスレッドを、指定されたミリ秒だけ一時停止させる

## Executorフレームワーク
スレッドプールを使うためのフレームワーク

### 参考
- [Chapter3/Executor.md](Executor.md)

## スレッドの同期
java.util.concurrent.CycleBarrierクラスを使用する

### 参考
- [Chapter3/Executor.md](CycleBarrier.md)

## 排他制御
synchronizedキーワードを使用する<br>
メソッド宣言に使用する場合と、メソッド内の一部の処理だけを対象に使用する場合の2種類が存在する

### java.util.concurrent.atmicパッケージ
原始性を担保するクラスに関するパッケージ

- AtmicBoolean
- AtmicInteger
  - addAndGet(int num)<br>
  引数で渡された値分、valueを加算する
- AtmicLong
- AtmicReference

### java.util.concurrent.locks.ReentrantLockクラス
複数スレッドにまたがる排他制御を実現することができる<br>
lockの取得、unlockなどを行うことで、メソッドを呼び出す順番をしてすることができる

## スレッドセーフ
同時並行で実行しても問題が発生しないこと

### 配列
|パッケージ|クラス|スレッドセーフか|特徴|
|----------|----------|----------|----------|
|java.util|ArrayList|スレッドセーフではない|読み出しをしている際に要素の追加や削除が行われると、例外がスローされる<br>java.util.ConcurrentModificationExeption|
|java.util.concurrent|CopyOnWriteArrayList|スレッドセーフ|読み込みと書き出しを同時に行っても例外が発生しない|
|java.util|Vector|スレッドセーフ|すべてのメソッドがsynchronizedで修飾されている<br>読み出しと書き出しを同時に行うと、例外がスローされる<br>java.util.ConcurrentModificationExeption|
