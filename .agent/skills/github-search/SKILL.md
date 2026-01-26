---
name: github-search
description: 根据用户需求描述搜索 GitHub 上的相关开源项目。当用户说"找XX工具"、"有没有XX库"、"推荐XX项目"或描述某种功能需求时触发。
---

# github-search

## 概述

此 skill 帮助用户根据自然语言需求描述，在 GitHub 上查找相关的开源项目、工具或库。通过智能构造 GitHub 搜索查询，找到最匹配的仓库。

## 触发时机

当用户请求以下内容时触发此 skill：
- "找XX工具"、"有没有XX库"、"推荐XX项目"
- "需要做XX，有什么推荐吗"
- 描述功能需求寻找开源解决方案
- 查询特定技术栈或领域的项目

## 实现方案

### GitHub Search API

**API 端点**：`GET https://api.github.com/search/repositories`

**核心参数**：
- `q`：搜索查询（支持组合 qualifiers）
- `sort`：stars / forks / help-wanted-issues / updated
- `order`：desc / asc
- `per_page`：10-100

### 搜索查询构造策略

根据用户需求描述，构造包含以下元素的查询：

| 需求类型 | Qualifier 示例 |
|---------|---------------|
| 编程语言 | `language:python` / `language:javascript` |
| 话题 | `topic:ml` / `topic:web-framework` |
| Star 数 | `stars:>1000` / `stars:100..1000` |
| 更新时间 | `pushed:>2024-01-01` |
| 许可证 | `license:mit` / `license:apache-2.0` |
| 仓库名匹配 | `name:cli` / `name:api` |

**查询优先级排序**：
1. 精确关键词匹配（name:）
2. 话题匹配（topic:）
3. 语言筛选（language:）
4. 质量筛选（stars: > 活跃项目阈值）

## 执行步骤

1. **理解需求**：提取核心关键词、技术栈、功能描述
2. **构造查询**：组合关键词和 qualifiers 优化搜索结果
3. **执行搜索**：调用 GitHub Search API
4. **解析结果**：提取仓库名称、描述、star 数、语言、更新时间
5. **格式化输出**：以中文表格呈现，并提供项目链接

## 输出格式

使用中文表格展示：

| 仓库 | Stars | 语言 | 更新时间 | 描述 |
|------|-------|------|----------|------|
| [owner/repo](url) | 数量 | lang | 日期 | 简要描述 |

**可选补充**：
- 如果结果质量不高，添加提示："如需更精确的结果，请补充：编程语言、最低 star 数、许可证要求等"

## 搜索质量优化

### 默认筛选规则

除非用户明确要求，否则默认应用：
- `stars:>50`：排除不活跃项目
- `pushed:>2024-01-01`：排除长期未更新项目

### 按需求类型调整

| 用户需求 | 查询策略 |
|---------|---------|
| 找 CLI 工具 | `name:cli stars:>100 pushed:>2024-01-01` |
| 找 Web 框架 | `topic:web-framework language:javascript` |
| 找 AI/ML 项目 | `topic:machine-learning language:python stars:>500` |
| 找生产级库 | `stars:>1000 license:mit OR license:apache-2.0` |

## 错误处理

| 场景 | 处理方式 |
|------|----------|
| 无搜索结果 | 提示用户放宽筛选条件，或尝试不同关键词 |
| API 限流 | 提示认证或稍后重试，建议使用 GitHub token |
| 结果相关性低 | 建议用户补充更具体的需求描述 |

## 示例工作流

### 示例 1：找 Python PDF 处理库

**用户输入**："找一个处理 PDF 的 Python 库"

**执行**：
```bash
curl "https://api.github.com/search/repositories?q=pdf+language:python+stars:>100&sort=stars&order=desc&per_page=10"
```

**输出**：
| 仓库 | Stars | 语言 | 描述 |
|------|-------|------|------|
| [py-pdf/pypdf](https://github.com/py-pdf/pypdf) | 12,345 | Python | 纯 Python PDF 工具包 |
| [mstamy2/PyPDF2](https://github.com/mstamy2/PyPDF2) | 8,901 | Python | PDF 操作库 |

### 示例 2：找 React 状态管理方案

**用户输入**："React 状态管理有什么推荐"

**执行**：
```bash
curl "https://api.github.com/search/repositories?q=topic:state-management+language:javascript+stars:>1000&sort=stars&order=desc&per_page=10"
```

**输出**：
| 仓库 | Stars | 语言 | 描述 |
|------|-------|------|------|
| [reduxjs/redux](https://github.com/reduxjs/redux) | 60,123 | JavaScript | JS 应用的可预测状态容器 |
| [facebook/flux](https://github.com/facebook/flux) | 30,456 | JavaScript | 应用架构 |

## 注意事项

- GitHub API 限流：60 次/小时（未认证），5000 次/小时（已认证）
- 搜索结果相关性取决于查询构造质量
- 某些项目描述可能不完整，需要结合仓库名称和 topics 判断
- 用户需求模糊时，主动询问补充信息（语言、star 阈值、许可证等）

## 技术限制

- 依赖 GitHub Search API 可用性
- 搜索结果按 GitHub 排序算法，可能不完全符合用户预期
- 无法搜索私有仓库（需认证）
- API 返回最多 1000 条结果（分页获取）
