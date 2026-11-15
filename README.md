# dsh-plugins

[haiwenxiang061-dev](https://github.com/haiwenxiang061-dev) 发布的 DeepSeek Harness 插件索引。

## 这个仓库是什么

`market.json` 是一份 **plugin market index format 1** 文档，DSH 的「插件市场 → 发现」标签页可以直接读取它。
仓库本身不含代码，插件在各自主仓库里：

| 插件 | 说明 | 仓库 |
| --- | --- | --- |
| `dsh-arcgispro` | 让 Harness 操作 ArcGIS Pro：87 个工具、1977 个 GP 工具、`.aprx` 工程与布局出图 | [dsh-arcgispro](https://github.com/haiwenxiang061-dev/dsh-arcgispro) |
| `dsh-envi` | 让 Harness 驱动 ENVI：26 个工具覆盖 295 个 ENVI 任务 | [dsh-envi](https://github.com/haiwenxiang061-dev/dsh-envi) |
| `dsh-prompt-complete` | 把输入框草稿改写成一个完整提示词并回填，附带 12 秒撤销 | [dsh-prompt-complete](https://github.com/haiwenxiang061-dev/dsh-prompt-complete) |

## 加入自己的插件市场

1. 侧边栏打开 **插件**，在 **实验性插件** 旁的 **更多** 里开启 **自定义插件市场**。
2. 切到 **发现** 标签，添加索引源：

```
https://haiwenxiang061-dev.github.io/dsh-plugins/market.json
```

备用 CDN 地址（jsDelivr 对 `@main` 走长期缓存，索引更新后可能延迟数小时，所以只作兜底）：

```
https://cdn.jsdelivr.net/gh/haiwenxiang061-dev/dsh-plugins@main/market.json
```

索引不允许重定向，必须写最终地址。

## 直接安装（不用市场）

```bash
dsh plugin --profile desktop add dsh-arcgispro
dsh plugin --profile desktop add dsh-envi
dsh plugin --profile desktop add dsh-prompt-complete
```

上面三条已在空 profile 上实测通过（pnpm 11.28.5，输出 `+ dsh-arcgispro 0.1.1` 等）。
三个包都已在 npm 上发布，索引条目的 `source` 是 `{"type":"npm"}`。

不走 npm 时改用钉住 tag 的 tarball 地址（HTTP 下载，不依赖 npm registry）：

```bash
dsh plugin --profile desktop add https://codeload.github.com/haiwenxiang061-dev/dsh-arcgispro/tar.gz/refs/tags/v0.1.1
dsh plugin --profile desktop add https://codeload.github.com/haiwenxiang061-dev/dsh-envi/tar.gz/refs/tags/v0.8.2
dsh plugin --profile desktop add https://codeload.github.com/haiwenxiang061-dev/dsh-prompt-complete/tar.gz/refs/tags/v1.0.1
```

每个仓库的 Release 页还挂了同一份 `.tgz` 附件。

## 维护

`market.json` 里每个版本是 `source: { "type": "npm" }`（不带 `url`/`commit`），升级时改 `version` 与 `publishTimestamp` 两处；只有临时回退到 GitHub 源码装时才需要把它改成 `{ "type": "git", "url": ..., "commit": <40 位 sha> }`。
改完用 DSH 自带的校验器验一遍：

```bash
node -e "import('@deepseek-ai/dsh-plugin-market-index').then(m=>{const j=JSON.parse(require('fs').readFileSync('market.json','utf8'));const r=m.parseMarketIndex(j,'https://haiwenxiang061-dev.github.io/dsh-plugins/market.json');console.log(r.plugins.length+' 个插件')})"
```

## 许可

MIT。详见 [LICENSE](LICENSE)。
