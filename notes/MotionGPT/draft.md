## Abstract
虽然现有的预训练 LLM 快速发展，但是我们仍然难以构建一个统一处理语言和其它多模态数据的模型。

Human motion 和人类语言存在着较强的耦合关系（肢体语言？），所以我们试图通过将语言数据和大规模的运动模型相结合，完成 motion-language 的预训练。

主要方法是：通过离散向量量化方法对 human motion 进行建模，把 3D motion 转化成 motion tokens，生成过程类似于 word tokens 的生成。基于这种 motion 的词汇表，我们使用统一的方式对 motion 和 text 进行建模，也就是把 motion 看成一种特殊的语言。

受**提示学习**的启发，我们使用混合的 motion-language data 对预训练的模型进行微调（表现很好）。

> [!NOTE]
> Prompt Learning

## Introduction
相关的工作（MDM，MLD，MotionCLIP，TM2T）将 motion 和 language 看成不同的模态（这需要严格的 motion-text 对）。由于其监督信号高度依赖具体任务，这些方法难以有效泛化到未见过的任务或者数据，原因在于它们缺乏对 motion 与 language 之间关系的理解。

本文提出了一种统一的 motion-language 框架（MotionGPT）。
- 使用 VQ-VAE 去构建 motion 词汇表，把 motion 转化为一系列的 token，这些 token 被送入与训练的语言模型，学习它的底层语法与句法结构。
- 使用 two-stage 训练策略，首先在 motion dataset 上去预训练 language model，让它学底层语法与句法，然后在一个 instruction-dataset（包含 textual descriptions 和 motion data） 上微调这个 language model。SOTA on text-to-motion, motion-to-text, motion prediction, motion in-between。

Contributions: 
- 提出了一种统一的 motion-language 生成模型，**将 motion 看成一种语言**，将自然语言模型引入到运动相关的生成任务中，通过单一模型完成多种不同的运动任务。
- 提出了一种结合指令微调的训练方案。（instruction dataset）
- 提出一种 benchmark （text-to-motion, motion-to-text, motion prediction, motion in-between）