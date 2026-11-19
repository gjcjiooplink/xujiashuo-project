# library — 已筛选视频仓库

这个目录是**独立 git 仓库**，存放通过去重校验的猎奇视频。

```
library/
├── videos/          视频本体（每个文件已通过 SHA-256 去重）
├── .dedupe/         哈希索引缓存（本地，已 gitignore，可随时重建）
├── .gitignore
└── README.md
```

## 视频是怎么进来的

由 `tools/ingest.py` 从 `../inbox/` 移入。手动拖进来也没问题，
下次跑 `ingest.py` 时会自动为它补算哈希并写进索引。

## 提交

```powershell
python ..\tools\publish.py              # 只提交到本地 git
python ..\tools\publish.py --push       # 提交并推到远端
```

## 关于大文件

视频体积大，git 仓库会涨得很快。如果总量预计超过几百 MB，
建议接入 Git LFS（`.gitattributes` 已预置规则，取消注释即可启用）：

```powershell
git lfs install
git lfs track "videos/**/*.mp4" "videos/**/*.mkv" "videos/**/*.mov"
git add .gitattributes
```

## 索引文件

`.dedupe/index.json` 记录每个视频的 `sha256 / size / mtime`。
删掉它不会丢数据——下次运行会重新计算，只是慢一点。
