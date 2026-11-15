# podic tts

Podic 词典端侧 TTS 语音模型的分发清单仓。App 内「设置 → 语音引擎」从这里检测版本并下载模型（按需，不进安装包）。

## 清单地址

App 固定读取最新 release 的资产：

```
https://github.com/podic-dictionary/tts/releases/latest/download/tts-metainfo.json
```

## 字段约定

- `schema_version`：清单协议版本（当前 1）
- `catalog_revision`：清单整体修订号（展示/排查用）
- `models[].model_id`：稳定主键（如 `supertonic-3-int8`），更新只涨 `revision`
- `models[].revision`：同一 model_id 下严格递增，App 以此判断更新
- `models[].package`：压缩包直链 + sha256 + `archive_root`（解包时剥掉的外层目录）
- `models[].files[]`：解包后逐文件 size/sha256，安装时强制校验
- `models[].config`：原生壳构造引擎所需的文件名映射（按 family 结构不同）
- `models[].licenses[]`：模型与运行时许可（公开分发必须附）

## 添加/更新模型

1. 更新 `tts-metainfo.json`（新模型加条目；已有模型涨 `revision` 并换包 sha256），提交
2. 打新 tag（如 `catalog-v2`）发 release，资产上传新的 `tts-metainfo.json`

`releases/latest` 始终指向最高 tag，App 侧无需任何改动。
