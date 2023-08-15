# Executorフレームワーク
スレッドプールを使うためのフレームワーク

## スレッドプール
Java SE 5から導入された仕組み<br>
複数個のスレッドをあらかじめ用意し、タスクを振り分ける

## フレームワーク内の構成
- インターフェース
  - java.util.concurrent.Excutor
  - java.util.cuncurrent.ExecutorService
  - java.util.cuncurrent.ScheduledExecutorService
- インターフェースを取得するためのクラス
  - java.util.concurrent.Execurots

## Executorsクラス

### ファクトリメソッド
- newSingleThreadExecutor<br>
新しい1つのスレッドを作ってプールしているExecutorServiceを作成する
- newFixedThreadPool<br>
作成したいスレッドの数を引数に受け取り、複数スレッドを保持したExecutorServiceを作成する
- newCachedThreadPool<br>
必要に応じてスレッドを増減させるスレッドプールを作成する<br>
60秒の間利用されないスレッドは削除される
- newSingleThereadScheduledExecutor<br>
新しい1つのスレッドを作ってプールしているScheduledExecutorServiceを作成する
- newScheduledThreadPool<br>
作成したいスレッドの数を引数に受け取り、複数のスレッドを保持したScheduledExecutorServiceを作成する

## java.util.concurrent.ScheduledExecutorService
メソッドを定期的に実行するためのスレッドを作成する

### メソッド
- schedule(Runnable command, long delay, java.util.conccurent.TimeUnit unit)<br>
commandで渡された処理を指定された時間が経過した後に1度だけ実行する
- scheduleAtFixedRate(Runnable command, long initialDelay, long period, java.util.concurrnet.TimeUnit unit)<br>
commandで渡された処理を、initialDelay経過後から定期的に実行する<br>
2回目以降の実行は、前の実行が終わっておりかつインターバルが終了している時に行われる
- scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, java.util.concurrent.TimeUnit unit)<br>
commandで渡された処理を、initialDelay経過後から定期的に実行する<br>
2回目以降の実行は、前回の実行が終わってからdelay経過後に実行される

## java.util.concurrent.Featureインターフェース
Java SE 5で導入された<br>
mainメソッドからスレッドの状態を確認するためのもの

### オブジェクトの取得方法
ExecutorServiceやScheduledExecutorServiceにタスクを登録した際の戻り値で取得する

### メソッド
- get()<br>
スレッドの処理結果を受け取る<br>
  - java.util.concurrent.Runnableインターフェースのrunメソッドを渡した場合、タスク終了時に得られる値はnullがデフォルトである<br>
  (タスクを登録する際のsubmitの第二引数に戻り値を指定すると、その値を返すことも可能)
  - タスクの定義の際に、java.util.concurrent.Callableインターフェースを使うと、null以外の戻り値を受け取ることができる
