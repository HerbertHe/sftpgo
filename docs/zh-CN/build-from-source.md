# 从源代码构建 SFTPGo

下载源码并运行 `go build`。

下面的构建 tag 是可用的：

- `nogcs`，禁止 Google 云存储后端，默认开启
- `nos3`，禁止 S3 兼容对象存储后端，默认开启
- `noazblob`，禁止 Azure Blob 存储后端，默认开启
- `nobolt`，禁止 Bolt 数据提供程序，默认开启
- `nomysql`，禁止 MySQL 数据提供程序，默认开启
- `nopgsql`，禁止 PostgreSQL 数据提供程序，默认开启
- `nosqlite`，禁止 SQLite 数据提供程序，默认开启
- `noportable`，禁止便携模式，默认开启
- `nometrics`, 禁止 Prometheus metrics，默认开启

如果没有构建 tag 被指定，构建将包含所有的默认特性。

可选的 [SQLite 驱动](https://github.com/mattn/go-sqlite3 "go-sqlite3") 是一个 `CGO` 包，因此在构建时需要 `C` 编译器。
在 Linux 和 MacOS，编译器安装非常简单或者早都已经安装过了。在 Windows 中，你需要下载 [MinGW-w64](https://sourceforge.net/projects/mingw-w64/files/) 并且在它的命令执行界面构建。

编译器只是构建时依赖。在运行时并不需要。

版本信息，诸如 git commit、构建时间，可以在构建时嵌入设置以下字符串变量:

- `github.com/drakkan/sftpgo/v2/version.commit`
- `github.com/drakkan/sftpgo/v2/version.date`

例如，你可以通过下面的命令进行构建：

```bash
go build -tags nogcs,nos3,nosqlite -ldflags "-s -w -X github.com/drakkan/sftpgo/v2/version.commit=`git describe --always --dirty` -X github.com/drakkan/sftpgo/v2/version.date=`date -u +%FT%TZ`" -o sftpgo
```

你可以获取包含 git commit、构建时间和可用特性的版本号，如下面这样：

```bash
$ ./sftpgo -v
SFTPGo 0.9.6-dev-b30614e-dirty-2020-06-19T11:04:56Z +metrics -gcs -s3 +bolt +mysql +pgsql -sqlite +portable
```
