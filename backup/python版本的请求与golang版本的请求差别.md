## 对于query的escape处理策略不同
* python 如果使用 `urllib.parse.quote` 默认会有一个safe 其中对于 `abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_.-~/` 这些字符默认是不进行转换的.
* golang 使用 ` url.QueryEscape` 默认是会把所有的符号和字符都会序列化 因为按照要求http标准的要求是要保证query没有特殊字符的
* 但是这点在进行一些爬虫业务时候特别不友好 其中以 `/` 被序列化最为致命 所以在golang中可以使用以下方法进行序列化保证与python的一致
```golang
func CustomQueryEscape(s string) string {
	// 定义不需要转义的字符
	safeChars := "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_.-~/"

	// 创建一个字符串构建器
	var escaped strings.Builder

	// 遍历字符串中的每个字符
	for _, char := range s {
		// 如果字符在 safeChars 中，则直接添加到结果中
		if strings.ContainsRune(safeChars, char) {
			escaped.WriteRune(char)
		} else {
			// 否则，使用 url.QueryEscape 对字符进行编码
			escaped.WriteString(url.QueryEscape(string(char)))
		}
	}

	return escaped.String()
}
```
## 请求的区别
* 如果使用requests 默认就是http1 因为没有http2
* 而golang默认就是http2 自动协商的