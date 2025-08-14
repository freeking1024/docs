# 如何写好 Prompt

[如何写好 Prompt-李继刚](https://www.lijigang.com/posts/chatgpt-prompt-structure/)

## 结构化

- 结构

  结构中的信息, 可以根据自己需要进行增减, 从中总结的常用模块包括:

  - **#Role:** : 角色指定，限制 GPT 的领域输出

  - **##Profile author/version/description**: Credit 和 迭代版本记录

  - **##Goals**: 一句话描述 Prompt 目标, 让 GPT Attention 聚焦起来

  - **##Constrains**: 描述限制条件, 其实是在帮 GPT 进行剪枝, 减少不必要分支的计算

  - **##Skills**: 描述技能项, 强化对应领域的信息权重

  - **##Workflow**: 重点中的重点, 你希望 Prompt 按什么方式来对话和输出

  - **#Initialization**: 冷启动时的对白, 也是一个强调需注意重点的机会
