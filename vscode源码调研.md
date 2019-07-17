# typescript / electron  vscode源码探究

> So begins a new age of knowledge

---

    Tip: 本文假设你已经有electron基础知识，如果你从头开始学习，请查看我的另一篇介绍electron基础的文章: [electron基础学习](https://gitpress.io/@amber/electron%E5%9F%BA%E7%A1%80)
    
---

## 前言

  目的是做一个方便公司业务开发、支持公司特定DSL可视化编辑等需求，决定基于vscode(版本:1.3.3)(下文统一简称**vsc**)进行修改，下文是vscode源码部分的解读

---

### 需求
1. 制作一款特定的可视化编辑器，用于编辑DSL
2. 新增向导功能，用于新建资源
3. 新增新的扩展点，允许用户在新的扩展点直接新增自定义编辑器等

---