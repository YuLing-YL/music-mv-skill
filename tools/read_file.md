# 工具名称
read_file

描述：
按需读取已落盘的中间产物（节拍 json 的摘要段、分镜表、自检报告），只把需要的内容取回上下文。是「数据落盘 + 按需读取」模式的另一半。

入参：
| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| path | string | 是 | 文件绝对路径 |
| offset | integer | 否 | 起始行号（从 1 开始），配合 limit 做局部读取 |
| limit | integer | 否 | 最多读取行数，默认 200 |
| json_pointer | string | 否 | JSON 路径（如 /summary/sections），只取指定子树 |

返回值（JSON结构）：
```json
{
  "ok": true,
  "path": "./output/music_mv/beats.json",
  "content": "...",
  "total_lines": 486,
  "truncated": true,
  "error": null
}
```

约束与注意事项：
- 读取节拍 json 时优先用 json_pointer 取 summary/sections，避免拉回整个逐拍数组撑爆上下文。
- truncated=true 时说明内容被截断，不要基于截断数据下结论，缩小范围再读。
- 文件不存在返回 ok=false，不要重试同一路径，先确认 Step 1 是否已落盘。
- 本工具只读；任何写操作走 save_file。
