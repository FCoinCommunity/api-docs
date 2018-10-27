# エラーコード

エラーコード | 内容解釈
---------- | -------
400 | Bad Request --  誤リクエスト
401 | Unauthorized -- API key 或いは署名、タイムスタンプに誤りがあります
403 | Forbidden -- アクセス不可
404 | Not Found -- リクエストされたリソースが見つかりませんでした
405 | Method Not Allowed -- 使用されるHTTPメソッドは、要求されたリソースには適用されません
406 | Not Acceptable -- リクエストされたコンテンツのフォーマットはJSONではありません
429 | Too Many Requests -- 負荷がオーバーロード、リクエスト頻度を下げてください
500 | Internal Server Error -- サービスの内部エラーが発生、後でもう一度お試しください
503 | Service Unavailable -- サービスが利用できません、後でやり直してください
