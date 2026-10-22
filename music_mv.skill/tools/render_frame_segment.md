# 工具名称
render_frame_segment

描述：
按单个镜头的分镜描述渲染一段动画视频片段（无音轨），每段渲染后立即执行画风一致性与卡点对齐自检。对应流水线 Step 3。

入参：
| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| shot_id | string | 是 | 镜头ID，如 SH004 |
| start_sec / end_sec | number | 是 | 镜头起止时间（秒） |
| style_locked | string | 是 | 定稿画风描述，从 generate_storyboard 返回值透传，保证一致性 |
| visual_prompt | string | 是 | 该镜头画面内容 + 运镜 + 卡点触发动作的自然语言描述 |
| beat_marks | array | 否 | 该镜头内的卡点时刻（相对镜头起点的秒偏移），用于对齐触发动作 |
| resolution | string | 否 | 分辨率，需与全局一致 |
| output_path | string | 是 | 片段输出路径，如 ./output/music_mv/segments/SH004.mp4 |

返回值（JSON结构）：
```json
{
  "ok": true,
  "shot_id": "SH004",
  "output_path": "./output/music_mv/segments/SH004.mp4",
  "self_check": {
    "style_consistent": true,
    "beat_aligned": true,
    "max_beat_offset_sec": 0.04,
    "retry_count": 0
  },
  "error": null
}
```

约束与注意事项：
- 每次只渲染一个镜头；渲染前必须确认分镜已获用户确认（need_confirm 闸门）。
- 自检不过自动重渲染，最多 2 次；仍不过则降级为静态画面+慢推镜，并把降级项写回自检报告。
- beat_marks 任一卡点与节拍偏差 > 0.1s 判定 beat_aligned=false。
- 全部片段的分辨率、帧率、编码器必须与首个片段一致，否则 Step 4 拼接会失败。
