# Summarize ZJE Slides Skill


## ZJE-Slides.skill


在summarize-slides基础上为ZJE学生适配的课件总结skill，根据生物医学专业知识点的特点设计了总结格式，并加入了对图片的筛选排版功能。帮助你更快的复习！

示例如下

<img src="https://github.com/JoeChen1122/ZJE-Slides-Skill/blob/asset/%E5%AE%9E%E4%BE%8B%E6%96%87%E4%BB%B6.png" alt="神经科学原理笔记" width="500" />


### 安装

复制给 OpenCode / Claude Code / 其他支持 skills 的 LLM agent：

```text
Fetch and follow instructions from:
https://raw.githubusercontent.com/JoeChen1122/ZJE-Slides-Skill/main/INSTALL.md
```
### 必要配置
由于本项目涉及图像处理模块，所使用的 API 必须具备图像识别、图像理解或图像处理能力，例如 GPT 系列多模态模型、Kimi 视觉模型等。纯文本模型无法准确解析或处理图像内容，可能导致图像匹配错误、随机插入图片或生成结果与实际图像内容不符等问题。


### 快速开始

1. 让 Agent 按 `INSTALL.md` 完成安装与环境检查：

```text
https://raw.githubusercontent.com/JoeChen1122/ZJE-Slides-Skill/asset/INSTALL.md
```

2. 在会话中粘贴课件地址，直接描述需求即可，例如：

```text
把这个课件整理成考前复习资料，并生成pdf文件。
```

3. 也可以自然说明范围与风格，例如：

```text
只总结第 3 讲到第 5 讲，中文为主，保留英文术语，做成公式和概念速查表。
```

### 默认输入输出

- 默认输入为pdf格式的课件
- 默认交付物是整理好的 `.pdf` 文件
- 默认输出目录为源 PDF 同级的 `Summary - <pdf-stem>`
- 默认文风为中文主写，重要术语保留英文括注
- 重要的示意图，机制图会保留在相关文字后
- 默认内容应尽量带页码或紧凑页码范围，便于回查原课件

### 它适合做什么

- 课件复习资料
- 考点总结
- 公式 / 概念速查表
- 开卷考试用的 cheat sheet
- 带页码引用的课件 PDF 精简总结

### 与 `pdf` skill 的关系

- `summarize-slides` 必须与 `pdf` 配合使用
- `pdf` 负责 PDF 读取、提取、OCR 与页面内容确认等输入侧工作
- `summarize-slides` 负责全局通读、按讲次或主题分段、合并总结、覆盖性核查、写出 LaTeX，并在本地 LaTeX 环境可用时尝试编译最终 PDF
- 简单说：`pdf` 负责把课件读清楚，`summarize-slides` 负责把复习文档做完整

### 仓库内容

- `SKILL.md`：技能的说明，包含触发条件、工作流、边界与交付要求
- `INSTALL.md`：安装入口与环境检查说明
- `LICENSE`：本仓库当前发布版本附带的许可文件
- `README.md`：面向公开发布的中英双语介绍与使用说明

### 许可说明

本 skill 基于 [Li-Baichuan-James/summarize-slides-skill](https://raw.githubusercontent.com/Li-Baichuan-James/summarize-slides-skill) 修改和整理而来。

感谢原作者提供的工作流设计、课件总结规范和实践经验。本 README 在原有 skill 思路基础上，补充了更适合生物医学学生的整理方案。

---
