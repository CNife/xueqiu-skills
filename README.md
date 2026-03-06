# xueqiu-skills

雪球投资分析 AI 技能仓库 - 自动爬取关注时间线，AI 分析市场热点并生成 PDF 报告

## 快速开始

```bash
# 1. 检查环境（自动安装缺失的依赖）
sh skills/crawl-xueqiu-my-timeline/scripts/check-cdp.sh
sh skills/crawl-xueqiu-my-timeline/scripts/check-agent-browser.sh

# 2. 爬取最近 24 小时时间线
uv run skills/crawl-xueqiu-my-timeline/scripts/crawl_xueqiu_home_timeline_api.py

# 3. AI 自动读取、分析并生成 PDF 报告
```

## 依赖安装

### 必需依赖

| 依赖 | 用途 | 安装方式 | 参考文档 |
|------|------|---------|---------|
| **Node.js 22+** | 运行 agent-browser | `brew install node@22` (macOS) | [Node.js 官网](https://nodejs.org/) |
| **agent-browser** | 浏览器自动化控制 | `npm install -g agent-browser` | [GitHub](https://github.com/vercel-labs/agent-browser) |
| **Python 3.10+** | 运行爬取脚本 | 系统自带或使用 pyenv | [Python 官网](https://www.python.org/) |
| **uv** | Python 虚拟环境与包管理 | `curl -LsSf https://astral.sh/uv/install.sh \| sh` | [uv 官网](https://docs.astral.sh/uv/) |
| **Chromium/Chrome** | 访问雪球网站 | 系统浏览器或 `brew install chromium` | [Chromium](https://www.chromium.org/) |
| **mdpdf** | Markdown 转 PDF | `npm install -g mdpdf` | [mdpdf npm](https://www.npmjs.com/package/mdpdf) |
| **Bun** (可选) | 替代 npm 的工具 | `curl -fsSL https://bun.com/install \| bash` | [Bun 文档](https://bun.com/docs) |

### 一键检查脚本

项目提供自动化检查脚本，会**自动安装缺失的依赖**：

```bash
# 检查 Chrome Debug 模式（未运行则自动启动）
sh skills/crawl-xueqiu-my-timeline/scripts/check-cdp.sh

# 检查 agent-browser（未安装则自动安装）
sh skills/crawl-xueqiu-my-timeline/scripts/check-agent-browser.sh
```

## 可用技能

### crawl-xueqiu-my-timeline

爬取雪球首页关注的时间线，AI 分析投资观点并生成 PDF 报告。

**功能特点**：
- 自动过滤官方账号（上市公司、指数、ETF 等行情播报）
- 按发言人分组输出，发言数量降序排列
- AI 自动分析观点、识别市场热点
- 生成精美 PDF 投资分析报告

**使用场景**：每日投资要闻回顾、关注的大 V 动态追踪、特定事件期间的讨论分析

## 使用方法

```bash
# 爬取最近 24 小时（默认）
uv run skills/crawl-xueqiu-my-timeline/scripts/crawl_xueqiu_home_timeline_api.py
```

**脚本参数**：

| 参数 | 说明 | 示例 |
|------|------|------|
| `--hours N` | 爬取最近 N 小时 | `--hours 2` |
| `--days N` | 爬取最近 N 天 | `--days 7` |
| `--start-date` | 开始日期 (YYYY-MM-DD) | `--start-date 2026-03-01` |
| `--end-date` | 结束日期 (YYYY-MM-DD) | `--end-date 2026-03-06` |
| `-o, --output` | 输出文件名 | `-o my_timeline.md` |

**注意**：`--hours`、`--days`、`--start-date` 三个参数互斥，不能同时使用。

## 输出文件

### 爬取输出

爬取脚本生成：`home_timeline_YYYYMMDD_YYYYMMDD.md`

### AI 分析报告

AI 生成：`雪球时间线_YYYYMMDD_YYYYMMDD.pdf`

报告包含发言人统计、主要观点总结、涉及标的、情绪倾向、市场热点 TOP3、投资建议。

### PDF 输出

最终生成：`雪球时间线_YYYYMMDD_YYYYMMDD.pdf`

```bash
# 手动转换（AI 工具通常会自动执行）
bunx mdpdf 雪球时间线_YYYYMMDD_YYYYMMDD.md \
  --style=skills/crawl-xueqiu-my-timeline/assets/github-markdown.css
```

## 注意事项

### 1. 雪球账号登录

首次运行前需**手动登录雪球账号**：
- 脚本会自动打开浏览器并访问雪球首页
- 在浏览器中完成登录
- 登录信息会保存在 `browser_profiles/xueqiu_profile/` 目录

### 2. 验证码处理

如遇雪球验证页面：
1. 在浏览器中手动完成验证
2. 重新运行爬取脚本

### 3. 参数互斥规则

以下参数不能同时使用：
- ❌ `--hours` + `--days`
- ❌ `--hours` + `--start-date`
- ❌ `--days` + `--start-date`

## 常见问题

### Q: 脚本报错"未找到 Chrome"

**解决**：安装 Chromium 或 Chrome：
```bash
# macOS 安装 Chromium
brew install chromium
```

### Q: agent-browser 安装失败

**解决**：检查 Node.js 版本：
```bash
node --version  # 需 v22+
```

### Q: 爬取结果为空

**可能原因**：
1. 雪球账号未登录 → 手动登录浏览器
2. 时间范围太短 → 增加 `--hours` 或 `--days` 参数
3. 关注列表为空 → 在雪球 APP 关注投资大 V

### Q: PDF 生成失败

**解决**：检查 mdpdf 是否安装：
```bash
npm install -g mdpdf
```

## 参考资料

- **AI 编程指引**：[AGENTS.md](AGENTS.md) - AI 工具如何调用此技能
- **技能详细文档**：[skills/crawl-xueqiu-my-timeline/SKILL.md](skills/crawl-xueqiu-my-timeline/SKILL.md)
- **agent-browser**：https://github.com/vercel-labs/agent-browser
- **Bun 文档**：https://bun.com/docs

## 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件
