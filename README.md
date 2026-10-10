# ncqqpy-plugins

ncqqpy 机器人的**插件列表仓库**：`plugins.json` 是插件管理器
（框架里的 `plugins/plugin_manager`）的唯一可信来源。

## 怎么用

在机器人**私聊**里（仅超管可用）：

```
/plugin list                 看有哪些插件、装没装
/plugin install <插件名>      安装
/plugin update all           全部更新
/plugin uninstall <插件名>    卸载（进回收站，可找回）
```

插件管理器默认就从这个仓库的 `main` 分支读 `plugins.json`；换地址改
`config.json` 里的 `plugins.plugin_manager.manifest_url` 即可。

## 列表文件格式

```json
{
  "version": 1,
  "name": "ncqqpy 官方插件列表",
  "updated": "2026-10-10",
  "plugins": [
    {
      "name": "essential",
      "repo": "Green1423/ncqqpy-plugin-essential",
      "ref": "main",
      "version": "1.2.0",
      "description": "一句话介绍（/plugin list 里显示）",
      "tags": ["群管理"],
      "data": false
    }
  ]
}
```

| 字段 | 必填 | 说明 |
|------|------|------|
| `name` | ✅ | 插件标识，同时是本地目录名 `plugins/<name>/`；只允许字母数字与 `_ . -` |
| `repo` | ✅ | GitHub 仓库（`owner/repo`、网页链接、`git@github.com:...` 都认） |
| `ref` | | 分支 / tag / commit，缺省 = 仓库默认分支 |
| `version` | | 展示用版本号（实际版本以仓库里 `PluginBase.version` 为准） |
| `description` | | 一句话介绍 |
| `tags` | | 标签数组（或单个字符串） |
| `data` | | `true` 表示这插件有运行期 `data/`，卸载时保留 |

`version` 是列表文件的**格式**版本，当前只支持 `1`：填更大的数字时，
插件管理器会明确拒绝并要求先升级自己（而不是按老格式猜着解析）。

## 发布流程

```bash
# 在框架仓库里生成（含 README）到 dist/ncqqpy-plugins/
./venv-arm/bin/python -B tools/publish_plugins_repo.py

# 生成到指定目录，并推送到远端
./venv-arm/bin/python -B tools/publish_plugins_repo.py --out ~/ncqqpy-plugins \
    --remote git@github.com:Green1423/ncqqpy-plugins.git --push
```

当前列表里的插件：

- **essential** v1.2.0 — 群管理基础命令（禁言/踢人/解禁/群信息）+ 入群问候与定时撤回（[Green1423/ncqqpy-plugin-essential](https://github.com/Green1423/ncqqpy-plugin-essential)）
- **bupt_news** v1.0.0 — 北邮校内通知定时同步与推送（门户登录 + AI 摘要 + 表格出图）（[Green1423/ncqqpy-plugin-bupt-news](https://github.com/Green1423/ncqqpy-plugin-bupt-news)）
- **byrdocs** v1.0.0 — BYR Docs 资料检索与下载（按编号搜索 / 翻页 / 取文件 / 白名单）（[Green1423/ncqqpy-plugin-byrdocs](https://github.com/Green1423/ncqqpy-plugin-byrdocs)）
- **ai_chat** v1.0.0 — 多会话 AI 对话（上下文压缩 + 内置工具 + 联网搜索 + Markdown 出图）（[Green1423/ncqqpy-plugin-ai-chat](https://github.com/Green1423/ncqqpy-plugin-ai-chat)）

## 许可

MIT
