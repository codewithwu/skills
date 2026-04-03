> ## 文档索引
> 获取完整文档索引请访问: https://agentskills.io/llms.txt
> 在进一步探索之前，请先使用此文件发现所有可用页面。

# 规范

> Agent Skills 的完整格式规范。

## 目录结构

一个 skill 是一个目录，至少包含一个 `SKILL.md` 文件：

```
skill-name/
├── SKILL.md          # 必需：元数据 + 说明
├── scripts/          # 可选：可执行代码
├── references/       # 可选：文档
├── assets/           # 可选：模板、资源
└── ...               # 其他任意文件或目录
```

## `SKILL.md` 格式

`SKILL.md` 文件必须包含 YAML 前置数据，后接 Markdown 内容。

### 前置数据

| 字段              | 必需 | 约束条件                                                                                                   |
| --------------- | -------- | ----------------------------------------------------------------------------------------------------------------- |
| `name`          | 是      | 最多 64 个字符。仅限小写字母、数字和连字符。不得以连字符开头或结尾。                                               |
| `description`   | 是      | 最多 1024 个字符。非空。描述技能的功能及使用场景。                                                                |
| `license`       | 否       | 许可证名称或捆绑许可证文件的引用。                                                                             |
| `compatibility` | 否       | 最多 500 个字符。表示环境要求（目标产品、系统包、网络访问等）。                                                   |
| `metadata`      | 否       | 任意键值对，用于存储额外元数据。                                                                                |
| `allowed-tools` | 否       | 空格分隔的预批准工具列表。（实验性功能）                                                                         |

<Card>
  **最小示例：**

  ```markdown SKILL.md theme={null}
  ---
  name: skill-name
  description: 描述此技能的功能及使用场景。
  ---
  ```

  **包含可选字段的示例：**

  ```markdown SKILL.md theme={null}
  ---
  name: pdf-processing
  description: 提取 PDF 文本、填写表单、合并文件。处理 PDF 文件时使用。
  license: Apache-2.0
  metadata:
    author: example-org
    version: "1.0"
  ---
  ```
</Card>

#### `name` 字段

必填的 `name` 字段：

* 长度必须为 1-64 个字符
* 仅可包含 Unicode 小写字母 (`a-z`) 和连字符 (`-`)
* 不得以连字符开头或结尾
* 不得包含连续连字符 (`--`)
* 必须与父目录名称一致

<Card>
  **有效示例：**

  ```yaml  theme={null}
  name: pdf-processing
  ```

  ```yaml  theme={null}
  name: data-analysis
  ```

  ```yaml  theme={null}
  name: code-review
  ```

  **无效示例：**

  ```yaml  theme={null}
  name: PDF-Processing  # 不允许大写
  ```

  ```yaml  theme={null}
  name: -pdf  # 不能以连字符开头
  ```

  ```yaml  theme={null}
  name: pdf--processing  # 不允许连续连字符
  ```
</Card>

#### `description` 字段

必填的 `description` 字段：

* 长度必须为 1-1024 个字符
* 应描述技能的功能及使用场景
* 应包含特定关键词以帮助 agent 识别相关任务

<Card>
  **良好示例：**

  ```yaml  theme={null}
  description: 从 PDF 文件中提取文本和表格，填写 PDF 表单，合并多个 PDF。处理 PDF 文档或用户提及 PDF、表单、文档提取时使用。
  ```

  **不良示例：**

  ```yaml  theme={null}
  description: 帮助处理 PDF。
  ```
</Card>

#### `license` 字段

可选的 `license` 字段：

* 指定应用于技能许可证
* 建议保持简短（许可证名称或捆绑许可证文件名）

<Card>
  **示例：**

  ```yaml  theme={null}
  license: Proprietary. LICENSE.txt has complete terms
  ```
</Card>

#### `compatibility` 字段

可选的 `compatibility` 字段：

* 如果提供，长度必须为 1-500 个字符
* 仅在技能有特定环境要求时才应包含
* 可用于说明目标产品、所需系统包、网络访问需求等

<Card>
  **示例：**

  ```yaml  theme={null}
  compatibility: 专为 Claude Code（或类似产品）设计
  ```

  ```yaml  theme={null}
  compatibility: 需要 git、docker、jq 和互联网访问
  ```

  ```yaml  theme={null}
  compatibility: 需要 Python 3.14+ 和 uv
  ```
</Card>

<Note>
  大多数技能不需要 `compatibility` 字段。
</Note>

#### `metadata` 字段

可选的 `metadata` 字段：

* 字符串键到字符串值的映射
* 客户端可用此存储 Agent Skills 规范中未定义的额外属性
* 建议使用足够独特的键名以避免意外冲突

<Card>
  **示例：**

  ```yaml  theme={null}
  metadata:
    author: example-org
    version: "1.0"
  ```
</Card>

#### `allowed-tools` 字段

可选的 `allowed-tools` 字段：

* 空格分隔的预批准工具列表
* 实验性功能。支持情况因 agent 实现而异

<Card>
  **示例：**

  ```yaml  theme={null}
  allowed-tools: Bash(git:*) Bash(jq:*) Read
  ```
</Card>

### 正文内容

前置数据之后的 Markdown 正文包含技能说明。没有格式限制。编写任何有助于 agent 有效执行任务的内容。

建议章节：

* 分步说明
* 输入输出示例
* 常见边缘情况

请注意，agent 决定激活技能后会加载整个文件。考虑将较长的 `SKILL.md` 内容拆分到引用文件中。

## 可选目录

### `scripts/`

包含 agent 可执行的可执行代码。脚本应：

* 自包含或明确记录依赖
* 包含有用的错误消息
* 优雅地处理边缘情况

支持的语言取决于 agent 实现。常见选项包括 Python、Bash 和 JavaScript。

### `references/`

包含 agent 需要时可读取的额外文档：

* `REFERENCE.md` - 详细技术参考
* `FORMS.md` - 表单模板或结构化数据格式
* 领域特定文件（`finance.md`、`legal.md` 等）

保持各个[引用文件](#file-references)专注。agent 按需加载这些文件，因此较小的文件意味着更少的上下文使用。

### `assets/`

包含静态资源：

* 模板（文档模板、配置模板）
* 图片（图表、示例）
* 数据文件（查找表、模式）

## 渐进式披露

技能应结构化以高效使用上下文：

1. **元数据**（约 100 tokens）：所有技能的 `name` 和 `description` 字段在启动时加载
2. **说明**（建议 < 5000 tokens）：激活技能时加载完整的 `SKILL.md` 正文
3. **资源**（按需加载）：仅在需要时加载 `scripts/`、`references/` 或 `assets/` 中的文件

保持主 `SKILL.md` 少于 500 行。将详细参考材料移到单独的文件中。

## 文件引用

引用技能中的其他文件时，使用相对于技能根目录的相对路径：

```markdown SKILL.md theme={null}
详情请参阅[参考指南](references/REFERENCE.md)。

运行提取脚本：
scripts/extract.py
```

保持文件引用从 `SKILL.md` 起只深一层。避免深层嵌套的引用链。

## 验证

使用 [skills-ref](https://github.com/agentskills/agentskills/tree/main/skills-ref) 参考库验证您的技能：

```bash  theme={null}
skills-ref validate ./my-skill
```

这将检查您的 `SKILL.md` 前置数据是否有效并遵循所有命名规范。


Built with [Mintlify](https://mintlify.com).
