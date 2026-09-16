# 本机素材合同

只有配置缺失、素材路径失效或需要重新安装时读取本文件。

## 私有配置

配置文件位于 Skill 的 `state/local-config.json`，只存在于用户本机。源 Skill 不打包肖像、二维码、Logo、聊天原文或绝对私人路径。

首次配置：

```bash
python3 scripts/daily_signature_ops.py configure \
  --portrait "/absolute/path/to/portrait.png" \
  --logo "/absolute/path/to/logo.png" \
  --qr "/absolute/path/to/wechat-qr.jpg" \
  --output-dir "/absolute/path/to/output"
```

脚本会写入画布和当前基准坐标。配置文件不得复制到公开仓库。

## 素材角色

- `portrait.path`：imagegen 身份参考。保持本人脸部、发型、服装气质；替换输入背景。
- `qr.path`：最终联系入口。由 ffmpeg 以最近邻插值缩放后覆盖；生图模型不接触二维码。
- `logo.path`：官方横向 Logo。由 ffmpeg 等比使用源画布叠加；生图模型不重绘品牌字标。
- `output_directory`：只管理 `今日日签海报.png` 与 `朋友圈文案.md` 两个固定文件。

二维码缺失时不交付最终版。人物或 Logo 缺失时可以做待补素材的预览底图，但不得发布或替换上一组正式文件。

## 确定性命令

```bash
python3 scripts/daily_signature_ops.py compose \
  --base /path/to/generated-base.png \
  --output /path/to/.tmp-date-poster.png

python3 scripts/daily_signature_ops.py validate \
  --poster /path/to/.tmp-date-poster.png \
  --copy /path/to/.tmp-date-copy.md

python3 scripts/daily_signature_ops.py publish \
  --poster /path/to/.tmp-date-poster.png \
  --copy /path/to/.tmp-date-copy.md
```

`publish` 先验证，再替换固定文件；第二个替换失败时恢复上一组文件。
