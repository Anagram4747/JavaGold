# NIO.2
JSR 203: More New I/O APIs for the Java Platform<br> 
java SE 7で導入された<br>
java.io.Fileクラスでの以下のような課題の解決のために導入された
- UNIXでのシンボリックを適切に扱えない
- ディレクトリツリーの探索が手軽にできない
- ファイル属性の扱いやリネーム、非同期I/Oができない
- ファイルのコピーが簡単にできない

## ファイルとパスの扱い
- java.nio.file.Pathインターフェース<br>
ファイルやディレクトリへのパスを表すために使用
- java.nio.file.Filesクラス
ファイル操作をするために使用

## java.nio.file.Pathインターフェース

### インスタンス生成
```java
import java.io.File;
import java.nio.file.Paths;
import java.nio.file.Files;

// java.nio.file.Pathsクラスのgetメソッドを使用
Path path1 = Paths.get("sample.txt");
Path path2 = Paths.get("dir/sample.txt");
Path path3 = Paths.get("dir", "sampleDir", "sample.txt");

// java.io.FileクラスのtoPathメソッドを使用
Path path4 = new File("sample.txt").toPath();

// java.nio.file.Pathsクラスのresolveメソッドを使用
Path dir = Paths.get("dir", "subdir");
Path file = dir.resolve(Paths.get("data.txt"));
```

## java.nio.file.Filesクラス
java.io.Fileクラスにおけるファイル操作の部分を取り出したクラス

### メソッド
- static Path createFile(Path path, FileAttribute<?>... attrs)<br>
引数で渡されたパスのファイルを新規に作成する<br>
すでにファイルが存在している場合、java.nio.file.FileAlreadyExistsExceptionをスローする
- static FileTime getLastModifiedTime(Path path, LinkOption... options)<br>
ファイルの最終更新日時を調べるメソッド
- static Set\<PosixFilePermission> getPosixFilePermissions(Path path, LinkOption... options)<br>
ファイルのアクセス権を調べるためのメソッド<br>
    - POSIX<br>
    IEEEによって定められた標準規格<br>
    UNIX系OSとして備えるべき仕様を規定しているもの
- static void delete(Path path)<br>
ファイルを削除するためのメソッド
- static BufferedReader newBufferedReader(Path path, OpenOption... options)<br>
ファイル内のデータにアクセスするためのストリームを作成するメソッド
- static Stream\<Path> list(Path dir)<br>
ディレクトリ直下にあるディレクトリやファイルの一覧を取得するメソッド
- static Stream\<Path> walk(Path start, FileVisitOption... options)<br>
ディレクトリ内のファイルとディレクトリを再帰的に処理するメソッド

## java.nio.file.StandardOpenOption
ファイルを開く際のオプションを指定するための列挙型
|定数|意味|
|----------|----------|
|APPEND|追記モードでファイルを開く|
|CREATE|ファイルが存在しない場合は新しいファイルを作成する|
|CREATE_NEW|新しいファイルを作成し、ファイルがすでに存在する場合は失敗する|
|DELETE_ON_CLOSE|閉じるときに削除する|
|READ|読み込みアクセス用に開く|
