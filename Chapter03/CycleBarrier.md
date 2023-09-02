# java.util.concurrent.CycleBarrier
スレッドの同期を行うためのクラス

## コンストラクタ
CycleBarrier(int parties, Runnable barrierAction)<br>
partiesで指定された数のスレッドが待ち状態になった場合、バリアーアクションが実行される

## 使用方法
runメソッドやcallメソッドの中から、CycleBarrierクラスのインスタンスへの参照を利用し、awaitメソッドを実行する<br>
awaitメソッドを実行することで、スレッドが待ち状態になる
