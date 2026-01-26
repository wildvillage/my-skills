---
name: github-trending-retriever
description: 查询 GitHub 最近一周 star 数快速上升的开源项目。当用户想要发现热门开源项目或查找基于 star 增长的热门仓库时使用。
---

# github-trending-retriever

## 概述

此 skill 帮助用户发现 GitHub 上最近一周 star 数快速上升的仓库。通过解析 GitHub Trending 页面获取真实的 7 天 star 增长数据。

## 触发时机

当用户请求以下内容时触发此 skill：
- 查询 GitHub 热门项目 / 趋势项目
- 想要发现流行的开源项目
- 查找 star 增长快速的仓库
- 查看最近一周的 GitHub 趋势

## 实现方案

### 推荐方案：使用 webReader 解析 GitHub Trending 页面

**原因**：GitHub Search API 无法按 star 增长排序（只能按总 star 数排序），而 Trending 页面提供准确的 7 天增长数据。

**URL 格式**：`https://github.com/trending/{language}?since=weekly`

- `{language}`：可选，指定编程语言（如 python、javascript、typescript），留空则显示全部
- `since=weekly`：最近一周趋势

### 支持的时间范围
- `since=daily`：今日趋势
- `since=weekly`：本周趋势（默认）
- `since=monthly`：本月趋势

## 执行步骤

1. **构造 URL**：根据用户需求确定语言和时间范围
2. **获取页面**：使用 webReader 工具获取 GitHub Trending 页面内容
3. **解析数据**：从 HTML 中提取仓库名称、star 增长数、编程语言、描述
4. **格式化输出**：以中文表格形式呈现结果

## 输出格式

使用中文表格展示：

| 仓库 | Star 增长 (7天) | 编程语言 | 描述 |
|------|----------------|----------|------|
| [owner/repo](url) | 数量 | lang | 简要描述 |

## 错误处理

| 场景 | 处理方式 |
|------|----------|
| 网络请求失败 | 重试一次，仍失败则提示用户检查网络 |
| 解析失败 | 提示用户 GitHub 页面结构可能变化，建议稍后重试 |
| 无结果 | 提示用户尝试切换编程语言或时间范围 |

## 示例工作流

### 示例 1：查询全部语言本周趋势

```
1. URL: https://github.com/trending?since=weekly
2. 调用 webReader 获取页面
3. 解析并格式化输出中文表格
```

### 示例 2：查询 Python 项目本周趋势

```
1. URL: https://github.com/trending/python?since=weekly
2. 调用 webReader 获取页面
3. 解析并格式化输出中文表格
```

## 注意事项

- GitHub Trending 页面不需要认证即可访问
- 页面内容每小时更新一次
- 部分语言的 trending 页面可能因项目较少而结果有限
- 描述内容如有英文应保留原文或提供简要翻译

## 技术限制

- webReader 工具依赖网络连接
- GitHub Trending 页面结构可能变化，需要相应调整解析逻辑
- 不支持查询自定义时间范围（仅支持 daily/weekly/monthly）
