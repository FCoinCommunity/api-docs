# 错误代码

错误代码 | 含义解释
---------- | -------
400 | Bad Request -- 错误的请求
401 | Unauthorized -- API key 或者签名，时间戳有误
403 | Forbidden -- 禁止访问
404 | Not Found -- 未找到请求的资源
405 | Method Not Allowed -- 使用的 HTTP 方法不适用于请求的资源
406 | Not Acceptable -- 请求的内容格式不是 JSON
429 | Too Many Requests -- 请求受限，请降低请求频率
500 | Internal Server Error -- 服务内部错误，请稍后再进行尝试
503 | Service Unavailable -- 服务不可用，请稍后再进行尝试
