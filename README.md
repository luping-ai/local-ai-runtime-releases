# Local AI Hub — 安装包发布

本仓库**只有发布资产，没有源码**。

Local AI Hub（内部代号 Runtime）是 LocalKB 等客户端依赖的本地推理与知识库服务。
它**不开源**，以免费安装包形式发布。

---

## 下载

到 [Releases](../../releases) 页下载最新的 `LocalAIHub-<version>-setup.exe`。

| 平台 | 文件 |
| --- | --- |
| Windows x64 | `LocalAIHub-<version>-setup.exe`（约 31 MB） |
| macOS / Linux | 暂未提供 |

**模型不在安装包里**。装好后首次使用需要在管理后台下载模型（1~4 GB），
这一步需要联网。

---

## 安装前必须知道：这个包没有代码签名

你会看到 Windows 的拦截提示：

```
Windows 已保护你的电脑
Microsoft Defender SmartScreen 阻止了无法识别的应用启动。
    [更多信息] → [仍要运行]
```

**这是正常的**。我们没有购买代码签名证书（OV 约 $200/年），这是自觉的取舍，
不是遗漏。

点「更多信息」→「仍要运行」即可。如果你不想信任它，就别装 —— 这是合理的
选择，我们不会劝你关掉安全防护。

### 怎么自己验证没被篡改

每个 Release 的说明里都列了安装包的 **SHA-256**。下载后核对：

```powershell
Get-FileHash .\LocalAIHub-0.2.7-setup.exe -Algorithm SHA256
```

与 Release 说明里的值逐字比对。一致 = 文件与我们发布的完全一致。

**这比签名弱在哪**：签名能证明"是谁构建的"，哈希只能证明"与我公布的一致"。
如果这个 GitHub 仓库本身被攻破，两者都会失守。但哈希至少能挡住
"第三方下载站偷偷换包"这类中间人替换。

---

## 装完之后

1. 打开管理后台（安装完成后自动启动，或在开始菜单找 `Local AI Hub`）
2. 在模型页下载并加载一个对话模型（推荐 `qwen2.5-1.5b`，约 1.1 GB）
3. 需要语义检索的话，再配置一个 Embedding 模型（推荐 `multilingual-e5-small`）

后台地址与 token 在 `%LOCALAPPDATA%\LocalAIHub\runtime.json` 里。
客户端程序会自己读取它，不需要手工配置。

---

## 与 LocalKB 的关系

[LocalKB](https://github.com/luping-ai/local-knowledge-base) 是本服务的开源客户端
（Apache-2.0），作为「如何用本服务 + Python SDK 做应用」的参考实现。

**同一台机器只能有一份 Local AI Hub**（单实例锁 + 固定数据目录）。
所以 LocalKB 不会自己装一份，只连接已安装的本服务。

---

## 许可

本软件为闭源软件，免费使用但**不提供源码**。

使用条款见每一次 Release 附带的 `LICENSE.txt`。

**需要注意第三方组件的许可**：本软件捆绑了若干开源组件，其中
**PyMuPDF 是 AGPL-3.0**（或商业授权）。完整清单与义务见 Release 附带的
`THIRD-PARTY-NOTICES.txt`。

**模型文件的许可各不相同**：LFM 系列商用需年营收 < 1000 万美元，
Apache-2.0 系列的模型可自由商用。商业使用前请核对管理后台里每个模型
标注的许可。

---

## 这个仓库为什么单独存在

| 仓库 | 内容 | 可见性 |
| --- | --- | --- |
| `local-ai-runtime` | 源码（Go）、内部文档 | 私密 |
| `local-knowledge-base` | 桌面端源码 | 公开 |
| **`local-ai-runtime-releases`**（本仓库） | **只有发布资产** | 公开 |

把发布资产单独放一个公开仓库，是为了让源码仓库保持私密的同时，用户仍然
有一个稳定的下载地址与可见的版本历史。**本仓库不接受 PR** —— 没有代码。
