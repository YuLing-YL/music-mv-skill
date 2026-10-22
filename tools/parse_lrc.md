# 工具名称
parse_lrc

描述：
解析 LRC 歌词文件，输出逐行歌词时间轴（起止时间 + 歌词文本），并按歌曲段落标注归属，供分镜表「对应歌词」列对齐使用。

入参：
| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| lrc_source | string | 是 | LRC 文件的 URL 或本地绝对路径 |
| duration_sec | number | 是 | 音频总时长（秒），用于补齐末行结束时间 |
| audio_shturl | string | 否 | get_audio_shturl 返回的句柄，用于交叉校验 |

返回值（JSON结构）：
```json
{
  "ok": true,
  "lines": [
    { "index": 1, "start": 12.5, "end": 18.2, "text": "第一句歌词", "section": "主歌A" },
    { "index": 2, "start": 18.2, "end": 24.0, "text": "第二句歌词", "section": "主歌A" }
  ],
  "line_count": 42,
  "has_metadata": true,
  "error": null
}
```

约束与注意事项：
- LRC 缺失时禁止虚构歌词：返回 ok=false，由主流程询问用户改用纯器乐模式或补歌词。
- 支持增强格式（逐字时间轴 A2 扩展），逐字信息会折叠为行级起止。
- 输出必须通过 save_file 落盘为 lrc_timeline.json，上下文只保留行数与前几句预览。
