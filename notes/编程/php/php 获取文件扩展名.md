```
$filepath = '/path/to/file.ext';
echo pathinfo($filepath, PATHINFO_EXTENSION);
```

pathinfo 函数支持4种类型的返回：

- `PATHINFO_DIRNAME` – 目录
- `PATHINFO_BASENAME` – 文件名（含扩展名）
- `PATHINFO_EXTENSION` – 扩展名
- `PATHINFO_FILENAME` – 文件名（不含扩展名）
