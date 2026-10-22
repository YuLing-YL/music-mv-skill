# 示例 1：霓虹流体抽象 MV

## 场景

用户给了一首电子舞曲（120 BPM），想要一支没有具象角色、纯抽象视觉的霓虹流体 MV，卡点要准。

## 调用示例

```
用这首歌（https://example.com/tracks/synthwave_dream.mp3）做个MV，
霓虹流体那种抽象的感觉，青紫色调，卡点要准
```

技能解析结果（内部）：audio_url=该链接；style=霓虹流体抽象视觉，青紫主色调，发光流体为核心意象；need_confirm 默认 true；无剧情无人物 → 纯视觉向。

## 执行过程要点

1. **Step 1**：`get_audio_shturl` 资源化音频 → `split_audio_section` 解析出 BPM=120、4 段结构（前奏/主歌/副歌/尾奏），节拍数据落盘 `./output/music_mv/neon/beats.json`。无 LRC，询问用户后确认走纯器乐模式。
2. **Step 2**：`generate_storyboard` 生成分镜表 `storyboard.md`（22 个镜头），need_confirm=true，暂停。用户反馈「副歌部分再碎一点」→ 修改分镜：副歌镜头从 5s 一切改为 2.5s 一切，重落盘 v2 版。
3. **Step 3**：逐镜头调用 `render_frame_segment`，style_locked 全程透传同一句「霓虹流体，青紫主色调」。重拍触发「流体表面闪光+粒子爆发」，低频段「流体缓慢蠕动」。SH011 卡点偏差 0.14s 超标，自动重渲染 1 次后达标（0.05s）。
4. **Step 4**：`ffmpeg_concat` 拼接 22 段 + 原音频，总时长 213.2s（音频 213.1s，偏差 0.1s）。
5. **Step 5**：交付 final.mp4、storyboard_v2.md、beats.json、自检报告（22/22 通过，0 降级）。

## 适用场景

电子、氛围、后摇等无歌词或弱歌词音乐；纯视觉向 MV；抖音/B 站卡点视频。
