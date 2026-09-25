# Claude 环境检查 Skills

用于在 **macOS** 或 **Windows** 上审查 Claude Desktop、Claude Code、浏览器语言、网络路由、DNS/WebRTC 与代理断线行为。先只读检查；每一项设置修改前展示当前值、目标值、影响、回退和验证方法，得到设备所有者确认后才执行。

本仓库是独立的社区流程，**不是 Anthropic 官方项目，也不保证账号注册、订阅、KYC 或持续可用**。实际使用地点须符合 [Anthropic 支持地区政策](https://www.anthropic.com/supported-countries)。系统语言、时区和网络出口不能替代该政策。

## 选择 Skill

| 系统 | Skill | 状态 |
| --- | --- | --- |
| macOS | [claude-mac-readiness](skills/claude-mac-readiness/SKILL.md) | 在一台 Apple Silicon Mac 上做过安装、配置和连通性复测；故障断线未实测 |
| Windows 10/11 | [claude-windows-readiness](skills/claude-windows-readiness/SKILL.md) | 文档版，尚未在 Windows 真机做端到端验证 |

每个文件夹都有 SKILL.md、详细参考资料和报告模板。安装时只复制与你的系统对应的整个文件夹，保留 references 和 assets。

## 安装

**已有 Codex 或 Claude Code 的用户**，可把本仓库链接和对应的 Skill 路径交给助手，请它先审查文件，再安装到个人 Skills 目录。也可按下面步骤手动安装：

1. 在 GitHub 点击 Code → Download ZIP，解压仓库。
2. 将 skills/claude-mac-readiness 或 skills/claude-windows-readiness 整个文件夹复制到对应工具的个人目录：

| 工具 | macOS 目录 | Windows 目录 |
| --- | --- | --- |
| Codex | <code>~/.codex/skills/</code> | <code>%USERPROFILE%\.codex\skills\</code> |
| Claude Code | <code>~/.claude/skills/</code> | <code>%USERPROFILE%\.claude\skills\</code> |

3. 重新开启会话。Codex 请求中写“使用 $claude-mac-readiness”或“使用 $claude-windows-readiness”；Claude Code 输入 /claude-mac-readiness 或 /claude-windows-readiness。[Claude Code Skills 文档](https://code.claude.com/docs/en/skills)

## 推荐第一次运行

> 使用对应系统的 claude-readiness Skill，只读检查这台电脑的 Claude Desktop、Claude Code、Chrome 和当前代理。先报告证据和候选修改；每一项设置在我确认后再执行。不要主动断开网络。

之后可逐项决定是否修改时区、浏览器语言、DNS 或代理设置。修改后 Skill 应读回实际状态并复测。管理员密码、登录验证码、支付信息由设备所有者自己输入，不能发到聊天中。

## 远程协助

服务人员可以在自己的 Codex 中使用 Skill，通过客户授权的远程窗口检查客户电脑；客户也可以安装到自己的 Codex/Claude Code 中自行运行。远程模式下，服务人员本机的终端和网络结果**不能**当作客户电脑结果。每项设置需要客户本人确认，远控无法可靠执行时改为指导客户操作。

## 来源与边界

本流程独立归纳公开经验，包括[原始 X 长文](https://x.com/suyuan1711/status/2102744129235193975)，并以软件官方文档核对可验证的技术事实。仓库不含原帖截图、长段原文或其另行发布的清理 Skill 代码。第三方“IP 纯净度”评分不是封号概率；自动重连也不是断线期间完全阻断的证明。

本仓库采用 [MIT 许可](LICENSE)。你可以免费使用、修改和分发这些 Skill。若需要有人远程检查、配置、复测并提供回退报告，那属于单独的人工服务。
