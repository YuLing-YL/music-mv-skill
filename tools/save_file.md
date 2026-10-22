# 工具名称
save_file

描述：
把中间产物（节拍 json、歌词时间轴、分镜表、片段文件、自检报告）写入本地磁盘并返回落盘路径。流水线所有「数据落盘、不塞上下文」的动作都靠它完成。

入参：
| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| path | string | 是 | 目标文件绝对路径（目录不存在会自动创建） |
| content | string | 是 | 文件内容（文本或 JSON 字符串） |
| overwrite | boolean | 否 | 是否覆盖同名文件，默认 true |

返回值（JSON结构）：
```json
{
  "ok": true,
  "path": "./output/music_mv/beats.json",
  "size_bytes": 51230,
  "error": null
}
```

约束与注意事项：
- 路径必须落在 output_dir（默认 ./output/music_mv）或技能工作目录内，禁止写到系统目录。
- 节拍/歌词数据必须先落盘再进 Step 2，落盘路径要记录在自检报告里。
- 覆盖用户修改过的分镜表前，先保留旧版本为 storyboard_v{n}.md。
- 大文件（>10MB 二进制）建议用专用渲染/合成工具落盘，不要经此工具透传。
