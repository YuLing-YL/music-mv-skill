# 工具名称
ffmpeg_concat

描述：
用 ffmpeg 按镜头顺序拼接全部渲染片段，并混入原始音频，输出成品 mp4。对应流水线 Step 4。

入参：
| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| segment_list | array | 是 | 片段路径数组，按镜头ID顺序排列 |
| audio_path | string | 是 | 原始音频本地路径（cache 中的文件） |
| output_path | string | 是 | 成品输出路径，如 ./output/music_mv/final.mp4 |
| resolution | string | 否 | 统一输出分辨率，默认 1920x1080 |
| clip_start / clip_end | number | 否 | 片段模式：音轨只取该时间范围（秒，原曲时间轴），与分镜覆盖范围一致 |
| subtitle_path | string | 否 | 字幕文件（srt/ass）；仅在用户要求字幕时传入 |

返回值（JSON结构）：
```json
{
  "ok": true,
  "output_path": "./output/music_mv/final.mp4",
  "duration_sec": 210.6,
  "segment_count": 28,
  "audio_offset_sec": 0.08,
  "file_size_bytes": 158342144,
  "error": null
}
```

约束与注意事项：
- 拼接前核对：segment_list 数量 = 分镜镜头数；片段分辨率/帧率/编码器一致。
- 总时长与音频时长偏差 > 0.5s 时判定失败，逐段排查（常见原因：某片段丢帧、时间戳错乱）；片段模式与 clip_start~clip_end 时长核对。
- 片段模式用 ffmpeg 的 -ss/-to 精确截取音轨，截点对齐节拍，不得整体变速。
- 音画同步优先：音频流以 copy 或 -ss 精确对齐，不得整体变速。
- 未传 subtitle_path 时绝不加字幕（默认无字幕原则）。
- 依赖 ffmpeg；未安装时执行 `brew install ffmpeg`（macOS）或 `apt install ffmpeg`（Linux）。
