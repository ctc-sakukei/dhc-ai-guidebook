# Claude Code设计哲学
来自Claude Code作者Thariq Shihipar的文章 [Lessons from Building Claude Code: Seeing like an Agent](https://x.com/trq212/status/2027463795355095314)，以及笔者对于这篇文章的思考

本文的目的是让读者更好的使用Claude Code，以及提供设计Agent时的思路

## 渐进式披露 (Progressive Disclosure)

### 检索的演进
在构建上下文时，Claude Code早期使用 RAG 向量数据库来为 Claude 查找上下文。这样做的缺点是：这些信息是**被**放到上下文中的，而不是Claude主动需要的，所以上下文会有很多无用信息

因为Claude本身是支持搜索的，所以Claude Code后面变成了使用Grep工具，让Claude自行搜索文件、自主构建上下文

### 渐进式披露和Skill
随着Claude能力的提升，Claude Code发现只要提供合适的工具，Claude就越来越擅长自己构建所需的上下文，基于渐进式披露这一概念，Claude Code正式提出了Skill

Claude可以读取Skill文件，而Skill文件又可以继续引用其他文件，模型可以递归的读取他们

相比于把所有的内容都塞到上下文，渐进式披露的好处在于减少了上下文的开销，以及防止不必要的内容进入上下文

举个例子  
一个项目中有10个工具，每个工具的Prompt大概有1000个Token。如果把这些工具内容全放到上下文，在使用的时候会遇到两个问题
- 浪费了Token: 并不是每次任务都需要这10个工具的功能
- 会影响Claude的速度: Claude收到任务时，需要通读一遍十个工具所有的Prompt，然后再判断需要使用哪些工具，会严重拖慢响应速度

Skill的设计理念——使用简短的Prompt作为description，说明Skill的调用时机和作用  
如果Claude发现当前的任务涉及到某个Skill，再去完整的读取Skill的内容

### 保持`CLAUDE.md`整洁
基于渐进式披露这个理论，可以理解前面提到的[保持CLAUDE.md简洁](./3-ClaudeCode核心概念.md#claudemd遵守程度)这一原则。

精简这个文件的原则是：只保留项目的架构，和几条核心规则。对于代码规范，GIT规范等等可以放在别的文件中，然后通过引用的方式链接到`CLAUDE.md`中，从而让上下文保持整洁

### 让Claude自行搜索
目前Claude已经具备了搜索的能力  
所以在遇到问题时，相比于给Claude指定某些文件请调查这些文件，不如直接让Claude自行搜索  
指定文件会让Claude限定在这个范围内，如果指定的文件有误会让Claude产生幻觉

## 给模型合适的工具集
Thariq举了一个简单的例子  
当人遇到一个比较复杂的数学题，该怎么解决？
- 使用纸和笔——最简单，人人都会的方式，但是只能使用人脑运算
- 使用计算器——速度快了，但是前提是用户要会使用计算器
- 使用计算机——速度最快，效率高，但是用户要会写代码

这个例子阐明了一个道理：给模型能力匹配的工具，否则就会出现给一个不会写代码的人计算机——提升了成本，但是效果不如给他纸和笔；给一个只会纸笔运算的人纸笔、计算机、计算器，他纠结了半天发现最后还是只能使用纸笔——浪费了时间和思考，效果还是不如只给他纸和笔。

## 工具的替换和演进
早期Claude Code使用TodoWrite工具用来让Claude专注：列出当前的工作计划，每隔五次对话再次提醒Claude工作计划，保证Claude不会忘记

随着模型能力的进步，模型不再需要反复提醒待办事项也能清楚的知道自己应该做什么。反复的提醒Claude反而会让Claude死守任务清单而不是根据新的信息进行修改，导致TodoWrite工具反而成了让Claude正常工作的障碍

得益于Claude在sub-agent使用方面的改进，Claude Code团队使用了新的Task工具替代了TodoWrite，让sub-agent之间可以更好的协调工作
