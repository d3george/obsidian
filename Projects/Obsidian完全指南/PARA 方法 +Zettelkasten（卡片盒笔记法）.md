---
tags:
  - status/done
  - type/summary
  - topic/tools/obsidian
---

作为软件开发者，我们的知识输入源多、杂，且需要快速迭代和关联。Obsidian 的强大之处在于它的双向链接，但一个清晰的组织结构能让它发挥十倍的威力。

我们需要的不是一个“绝对”的系统，而是一个“**可演进、高弹性**”的系统。我推荐一套结合了 **PARA 方法** 和 **Zettelkasten（卡片盒笔记法）** 思想的混合系统。

这套系统分为“目录（文件夹）”和“标签”两个维度，它们各司其职：

  * **🗂️ 目录 (Folders): 解决“存放”问题**。它回答：“这篇笔记*是*什么？”（Is-A）。目录结构应该**极其简洁**，用来区分笔记的“状态”和“类别”。
  * **🏷️ 标签 (Tags): 解决“关联”问题**。它回答：“这篇笔记*关于*什么？”（Has-A）。标签是动态的、多维的，用来穿越目录，聚合主题。

-----

### 🗂️ 一、推荐的目录（文件夹）结构

我强烈推荐使用 **PARA** 方法作为你的顶级目录结构。它简洁、高效，并且完全符合开发者的工作流。

**PARA** 即：

  * `10_Projects` (项目)
  * `20_Areas` (领域)
  * `30_Resources` (资源)
  * `40_Archive` (归档)

为了更适配 Obsidian，我们可以稍作调整，增加一个入口和模板：

```
.
├── 00_Inbox (收件箱)
├── 10_Projects (项目)
├── 20_Areas (领域)
├── 30_Resources (资源)
├── 40_Archive (归档)
└── 99_Templates (模板)
```

#### 1\. `00_Inbox` (收件箱)

  * **用途**: 你的“临时大脑”。所有快速记录、未经整理的笔记都先扔在这里。
  * **规则**: 每天或每周清理一次。笔记要么被删除，要么被处理并移动到其他目录。
  * **推荐使用方法**： 使用浏览器插件[Obsidian Web Clipper](https://obsidian.md/clipper)一键捕获网页内容并导入 `Inbox`目录下

#### 2\. `10_Projects` (项目)

  * **用途**: 存放你**正在积极推进**、**有明确截止日期**的工作。
  * **举例**:
      * `Project_A_Backend_Refactor` (项目A的后端重构)
      * `Learn_Kubernetes_Sprint` (K8s 学习冲刺 - 把它当做一个有始有终的项目)
      * `Blog_Post_Go_Concurrency` (要写的博客文章)

#### 3\. `20_Areas` (领域)

  * **用途**: 存放你**长期负责、没有固定终点**的领域。这是你“保持标准”的地方。
  * **举例**:
      * `Work` (存放会议纪要、周报、团队标准)
      * `Career` (存放年度目标、简历、绩效评估)
      * `Health` (健身计划、食谱)
      * `Finance` (预算、投资理念)

#### 4\. `30_Resources` (资源)

  * **用途**: 你的“个人知识库”和“数字花园”。**这是你大部分学习笔记的最终归宿。**
  * **规则**: 这里的笔记是“主题驱动”的，而不是“来源驱动”的。
  * **推荐的子目录**:
      * `Topics` (技术主题):
          * `Programming/GoLang/`
          * `Programming/Python/`
          * `DevOps/Kubernetes/`
          * `Database/PostgreSQL/`
          * `Concepts/System_Design/`
      * `Sources` (原始材料 - 可选):
          * `Books/` (存放《DDIA》的读书笔记)
          * `Videos/` (存放某个 YouTube 视频的概要)
          * `Articles/` (存放某篇深度文章的总结)

> **核心技巧 (Zettelkasten)**:
> 你在 `Sources/Books/DDIA.md` 里记了读书笔记。但你学到的“Raft 一致性算法”这个**概念**，应该被提炼成一个**单独的笔记** `Topics/Concepts/Raft_Algorithm.md`。
> 然后，在 `Raft_Algorithm.md` 笔记里，你链接回 `Source: [[DDIA.md]]`。
> 这样，你的知识库是**以概念为核心**的，而不是以书本为核心。

#### 5\. `40_Archive` (归档)

  * **用途**: “冷藏库”。所有已完成的项目、不再相关的领域、过时的资源，都移到这里。
  * **规则**: 只进不出。这能让你的 `Projects` 和 `Areas` 保持清爽。

-----

### 🏷️ 二、推荐的标签（Tags）系统

标签的威力在于“跨目录”。**一个好的标签系统不应该重复你的目录名。**

既然你已经有了 `Topics/GoLang` 目录，就**不要**再用 `#GoLang` 标签了（这叫重复劳动）。标签应该用于描述“目录无法描述”的元信息。

我推荐使用“多维标签系统”，用前缀来区分：

#### 1\. `type/` (笔记类型)

这可能是最有用的标签，它告诉你“这篇笔记是什么形态”。

  * `#type/concept` (概念): 对一个知识点的原子化总结（如：`[[Go Goroutine]]`）。
  * `#type/summary` (摘要): 对一本书、一篇文章、一个视频的总结（如：《DDIA》读书笔记）。
  * `#type/snippet` (代码片段): 一段可复用的代码。
  * `#type/cheatsheet` (速查表): 某个工具或语言的快速参考。
  * `#type/meeting` (会议): 会议纪要。
  * `#type/question` (问题): 你在学习中遇到的一个具体问题（尚未解决）。
  * `#type/insight` (见解): 你的个人思考或灵感。

#### 2\. `status/` (工作流状态)

这能让你的笔记“动起来”，非常适合管理任务和学习进度。

  * `#status/todo` (待办): 需要处理。
  * `#status/in-progress` (进行中): 正在阅读或研究。
  * `#status/waiting` (等待): 依赖他人或外部条件。
  * `#status/done` (已完成): 任务完成。
  * `#status/review` (待复习): 学习过的知识，需要定期回顾（可配合Spaced Repetition插件）。

#### 3\. `source/` (信息来源)

用于快速过滤你从哪里学到的东西。

  * `#source/book`
  * `#source/article`
  * `#source/video`
  * `#source/podcast`
  * `#source/course`
  * `#source/conversation`

#### 4\. `topic/` (跨领域主题) - [可选]

如果一个主题太小，不值得创建一个目录，或者它横跨多个领域，就用标签。

  * `#topic/security`
  * `#topic/performance`
  * `#topic/management`

-----

### 🚀 三、实战演练：一个完整的流程

假设你今天在 YouTube 看了一个关于 "Go 语言并发" 的优质视频。

1.  **快速捕获**:

      * 你立即在 `00_Inbox` 中创建了一个新笔记 `Go并发视频.md`。
      * 你一边看一边随手记下要点，并打上标签：`#status/in-progress` `#source/video`。

2.  **整理与加工 (Processing)**:

      * 晚上，你清理收件箱。你决定深入消化这个笔记。
      * **第一步：移动 (Move)**。你把笔记重命名为 `Go Concurrency Patterns by X.md`，并移动到 `30_Resources/Sources/Videos/` 目录下。
      * **第二步：打标签 (Tag)**。你完成了笔记，把标签改为 `#type/summary` `#source/video`。

3.  **提炼知识 (Zettelkasten)**:

      * 视频里提到了三个核心概念：`Goroutines`, `Channels`, `Select`。
      * 你**创建了三个新的原子笔记**：
          * `30_Resources/Topics/Programming/GoLang/Goroutines.md`
          * `30_Resources/Topics/Programming/GoLang/Channels.md`
          * `30_Resources/Topics/Programming/GoLang/Select.md`
      * 在这些新笔记里，你用自己的话（而不是照搬）总结了这几个概念，并打上标签 `#type/concept`。
      * 在笔记的末尾，你添加了来源：`Source: [[Go Concurrency Patterns by X.md]]`。

4.  **应用 (Application)**:

      * 下周，你在做 `10_Projects/Project_A_Backend_Refactor` 时，需要用到并发处理。
      * 你在项目笔记 `Project_A_Backend_Refactor.md` 中写道：`"需要使用 [[Goroutines]] 和 [[Channels]] 来优化数据处理... (参考 [[Select.md]])"`。

**看，这个系统帮你完成了：**

1.  **捕获 (Inbox)**
2.  **整理 (Resources/Sources)**
3.  **提炼 (Resources/Topics)**
4.  **关联 (双向链接)**
5.  **应用 (Projects)**

这套系统兼顾了结构性（PARA）和灵活性（Tags + Links），非常适合需要持续学习和创造的开发者。

