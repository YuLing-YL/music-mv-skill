# 工具名称
split_audio_section

描述：
音频分析核心工具：基于 librosa 提取 BPM、节拍点、重拍、响度曲线（含低频能量），并切分歌曲段落（前奏/主歌/副歌/间奏/桥段/尾奏）。对应流水线 Step 1。

入参：
| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| audio_shturl | string | 是 | get_audio_shturl 返回的短链句柄 |
| output_json | string | 是 | 节拍数据落盘路径，如 ./output/music_mv/beats.json |
| sections_hint | array | 否 | 用户指定的段落切分提示（秒数组），可提高切分准确度 |

返回值（JSON结构）：
```json
{
  "ok": true,
  "output_json": "./output/music_mv/beats.json",
  "summary": {
    "bpm": 128.0,
    "duration_sec": 210.5,
    "beat_count": 449,
    "downbeat_count": 112,
    "sections": [
      { "name": "前奏", "start": 0.0, "end": 12.5 },
      { "name": "主歌A", "start": 12.5, "end": 40.0 },
      { "name": "副歌", "start": 40.0, "end": 68.0 }
    ]
  },
  "error": null
}
```

约束与注意事项：
- 完整节拍点、响度曲线只写入 output_json，返回值仅携带 summary 摘要，严禁把逐拍数组带回上下文。
- 段落切分对前奏短、结构复杂的歌可能不准，可让用户提供 sections_hint 或人工微调后再进 Step 2。
- 解析失败（文件损坏/格式异常）时返回 ok=false，不要降级为「估计节拍」。
- 依赖 librosa，缺失时先执行 `pip install librosa pydub`。
