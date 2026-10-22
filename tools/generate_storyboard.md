# 工具名称
generate_storyboard

描述：
读取节拍 json 与歌词时间轴，按固定卡点业务规则生成分镜表，输出符合 storyboard_template.md 表头的结构化分镜数据，并给出 need_confirm 标记。对应流水线 Step 2。

入参：
| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| beats_json | string | 是 | split_audio_section 落盘的节拍文件路径 |
| lrc_json | string | 否 | parse_lrc 落盘的歌词时间轴路径；纯器乐模式省略 |
| style | string | 是 | 画风定稿描述：画风 + 核心意象/角色 + 剧情线（可从用户的自由描述提取或智能推断） |
| plot | string | 否 | 剧情线（可选）；有剧情时分镜「画面内容」按剧情推进编排 |
| characters | string | 否 | 核心人物/角色设定（可选）；有人物时角色造型全程一致 |
| clip_start / clip_end | number | 否 | 片段模式：只在该时间范围（秒，原曲时间轴）内生成分镜；不传则整曲 |
| section | string | 否 | 片段模式：直接指定段落名（如「副歌」），自动换算为时间范围；与 clip_* 二选一 |
| resolution | string | 否 | 输出分辨率，默认 1920x1080 |
| output_md | string | 是 | 分镜表落盘路径，如 ./output/music_mv/storyboard.md |

返回值（JSON结构）：
```json
{
  "ok": true,
  "output_md": "./output/music_mv/storyboard.md",
  "shot_count": 28,
  "need_confirm": true,
  "style_locked": "neon_fluid：霓虹流体抽象视觉，青紫主色调，粒子为核心意象",
  "error": null
}
```

约束与注意事项：
- need_confirm=true 时主流程必须暂停等待用户确认，用户未确认前禁止调用任何渲染工具。
- 覆盖范围：整曲模式覆盖全曲；片段模式只覆盖 clip_start~clip_end 或 section 换算范围，时间轴保持原曲时间便于核对卡点。
- section 换算依据 beats_json 中的段落切分；指定段落不存在时返回 ok=false 并列出实际可用段落。
- 镜头切分遵循卡点规则：副歌 2~5s 加快切镜，主歌 8~15s，安静段慢运镜可放宽到 20s。
- 分镜一经用户修改，以修改版为准重新落盘，并以此为渲染唯一依据。
- 生成的分镜必须通过 self_check_rules.md 的节拍与节奏校验后再交给用户。
