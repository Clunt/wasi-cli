# WASI CLI World

[WebAssembly系统接口(WebAssembly System Interface)](https://github.com/WebAssembly/WASI)API提案。

### 当前阶段（Current Phase）

wasi-cli目前处于[第3阶段(Phase 3)][Phase 3]。

[Phase 3]:  https://github.com/WebAssembly/WASI/blob/main/Proposals.md#phase-3---implementation-phase-cg--wg

### 拥护者（Champions）

- Dan Gohman

### 可移植性标准（Portability Criteria）

WASI CLI必须有至少可以在Windows、macOS和Linux上通过测试套件(testsuite)的主机实现。

WASI CLI必须至少有两个完整独立的实现。

## 目录（Table of Contents）

- [介绍（Introduction）](#介绍introduction)
- [目标（Goals）](#目标goals)
- [非目标（Non-goals）](#非目标non-goals)
- [API详解（API walk-through）](#API详解api-walk-through)
  - [用例1](#用例1)
  - [用例1](#用例1)
- [详细设计讨论（Detailed design discussion）](#详细设计讨论detailed-design-discussion)
  - [stdout应该是`output-stream`吗？（Should stdout be an `output-stream`?）](#stdout应该是output-stream吗should-stdout-be-an-output-stream)
  - [stderr应该是`output-stream`吗？（Should stderr be an `output-stream`?）](#stderr应该是output-stream吗should-stderr-be-an-output-stream)
  - [环境变量应该作为`command`的参数吗？（Should environment variables be arguments to `command`?）](#环境变量应该作为command的参数吗should-environment-variables-be-arguments-to-command)
- [项目相关方利益 & 反馈（Stakeholder Interest & Feedback）](#项目相关方利益--反馈stakeholder-interest--feedback)
- [参考文献 & 致谢（References & acknowledgements）](#参考文献--致谢references--acknowledgements)

### 介绍（Introduction）

Wasi-cli是用于命令行界面(Command-Line Interface, CLI)环境的[world]提案。它提供了此类环境中常用的API，例如文件系统(filesystems)和套接字(sockets)，还提供了命令行功能(command-line facilities)如命令行参数(command-line arguments)、环境变量(environment variables)和标准输入输出(stdio)。

[World]: https://github.com/WebAssembly/component-model/blob/main/design/mvp/WIT.md#wit-worlds

### 目标（Goals）

Wasi-cli旨在用于以下用途：

 - 交互式命令行参数程序。

 - 使用文件系统、套接字和相关API并且期望使用CLI央视命令行启动的服务器。

 - 从标准输入读取并写入标准输入的流过滤器。

### 非目标（Non-goals）

Wasi-cli并不旨在彻底重新构想命令行界面程序的概念。虽然WASI整体上正在探索诸如[Typed Main]之类的想法，但wasi-cli坚持使用传统的字符串列表样式(list-of-strings style)的命令行参数。

[Typed Main]: https://sunfishcode.github.io/typed-main-wasi-presentation/

### API详解（API walk-through）

完整的API文档可以在[这里](command.md)找到。

TODO [演示如何使用此 API。]

#### [用例1]

[提供示例代码片段和图表，解释如何使用API来解决给定的问题]

#### [用例2]

[etc.]

### 详细设计讨论（Detailed design discussion）

#### stdout应该是`output-stream`吗？（Should stdout be an `output-stream`?）

对于服务器用例，标准输出（stdout）通常用作日志，通常不会有意义地阻塞、异步或易错。它只是程序发送消息并忽略的地方。
一种选择是为此类用例提供专用API，具有允许打印字符串的一个不返回`result`的单一函数，这意味着它永远不会失败。

然而，在实践中这只是较小的简化，专有云或边缘用例理想情况下应该迁移到比wasi-cli world更专业的领域，因为它们可以提供更大的简化，因此这似乎(提供专用API)并不值得。

#### stderr应该是`output-stream`吗？（Should stderr be an `output-stream`?）

这与stdout的问题类似，但对于标准错误（stderr），做这样的事情更有吸引力，因为stderr在许多类型的应用程序中以这种日志记录风格使用。。

然而，总体而言，保持stderr与stdout一致，并将我们对简化的期望集中于其他可以实现更大简化的领域似乎更为合适。

#### 环境变量应该作为`command`的参数吗？（Should environment variables be arguments to `command`?）

环境变量在某些非命令行用例中很有用，因此将它们保留为单独的导入意味着它们可以在没有`command`入口的world中被使用。

### 项目相关方利益 & 反馈（Stakeholder Interest & Feedback）

进入第3阶段之前的TODO。

[这应包括有兴趣实施该提案的实施者名单]

### 参考文献 & 致谢（References & acknowledgements）

wasi-cli的概念自发布以来已经开发了一年多，许多人贡献了有影响力的想法。非常感谢以下人士提供的宝贵反馈和建议：

- Luke Wagner
- Pat Hickey
