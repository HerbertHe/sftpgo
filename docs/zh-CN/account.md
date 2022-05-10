# 账户配置属性

请看一看 [OpenAPI schema](../openapi/openapi.yaml) 了解用户、文件夹和管理员域的确切定义。
如果你需要一个通过 Web 管理或者直接调用 `dumpdata` 导出转储文件的示例，你需要先获得一个 access token，比如：

```shell
$ curl "http://admin:password@127.0.0.1:8080/api/v2/token"
{"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOlsiQVBJIl0sImV4cCI6MTYxMzMzNTI2MSwianRpIjoiYzBrb2gxZmNkcnBjaHNzMGZwZmciLCJuYmYiOjE2MTMzMzQ2MzEsInBlcm1pc3Npb25zIjpbIioiXSwic3ViIjoiYUJ0SHUwMHNBUmxzZ29yeEtLQ1pZZWVqSTRKVTlXbThHSGNiVWtWVmc1TT0iLCJ1c2VybmFtZSI6ImFkbWluIn0.WiyqvUF-92zCr--y4Q_sxn-tPnISFzGZd_exsG-K7ME","expires_at":"2021-02-14T20:41:01Z"}

curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdWQiOlsiQVBJIl0sImV4cCI6MTYxMzMzNTI2MSwianRpIjoiYzBrb2gxZmNkcnBjaHNzMGZwZmciLCJuYmYiOjE2MTMzMzQ2MzEsInBlcm1pc3Npb25zIjpbIioiXSwic3ViIjoiYUJ0SHUwMHNBUmxzZ29yeEtLQ1pZZWVqSTRKVTlXbThHSGNiVWtWVmc1TT0iLCJ1c2VybmFtZSI6ImFkbWluIn0.WiyqvUF-92zCr--y4Q_sxn-tPnISFzGZd_exsG-K7ME" "http://127.0.0.1:8080/api/v2/dumpdata?output-data=1"
```

这个转储文件是 JSON 格式的，拥有包含用户、文件夹、管理员在内的所有 SFTPGo 数据。

这些属性被存储在配置的数据提供程序中。

SFTPGo 提供了以 bcrypt、pbkdf2、md5crypt 和 sha512crypt 存储的密码校验。对于 pbkdf2 支持的格式为 `$<algo>$<iterations>$<salt>$<hashed pwd base64 encoded>`，algo 为 `pbkdf2-sha1`、`pbkdf2-sha256`、`pbkdf2-sha512` 或 `$pbkdf2-b64salt-sha256$`。举个例子，使用 150000 次迭代和 E86a9YMX3zC7 作为盐的文本密码的pbkdf2-sha256 必须存储为 `$pbkdf2-sha256$150000$E86a9YMX3zC7$R5J62hsSq+pYw00hLLPKBbcGXmq7fj5+/M0IFoYtZbo=`。对于带有 b64salt 作为盐的 pbkdf2 变体是 base64 编码的。对于 bcrypt 格式必须被 go 语言的 crypto/bcrypt 包支持；举个例子，cost 为 14 的密码密钥必须被存储为 `$2a$14$ajq8Q7fbtFRQvXpdCq7Jcuy.Rx1h/L4J60Otx.gyNLbAYctGMJ9tK`。对于 md5crypt 和 sha512crypt 我们支持的格式在 `/etc/shadow` 中使用带有 `$1$` 和 `$6$` 前缀，如果你从 Unix 系统用户账户迁移是非常有用的。我们同样支持 Apache md5crypt (`$apr1$` 前缀)。使用 REST API 你可以发送由 bcrypt、pbkdf2、md5crypt 或 sha512crypt 哈希的密码，它们将如那样被存储。

如果你想使用已经存在的账户，有下面的选项：

- 在 SFTPGo 中导入你的用户。看一看 [转化用户](.../examples/convertusers) 脚本，它可以转化和导入来自于 Linux 系统的用户和 Pure-FTPd/ProFTPD 虚拟用户
- 你可以使用外部的认证程序
