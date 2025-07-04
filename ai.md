spring  ai 官网

- 使用构造函数

![image-20250428153145661](./ai.assets/image-20250428153145661.png)

# RAG

#### 过程：检索、增加和生成



# LLM

- 大语言模型的“大”主要体现几个方面在：
  - 一是参数数量庞大；
  - 二是训练数据量大；
  - 三是对计算资源需求高。
- 正是因为具备这些“大”的特点，很多先进的大语言模型参数不断增多，泛化性能愈发出色，在各种专门的领域输出结果也越来越准确

### 大型语言模型是零样本推理机（Large Language Models are Zero-Shot Reasoners）

- 预训练大型语言模型 (LLM) 广泛应用于自然语言处理 (NLP) 的众多子领域，并被公认为具有针对特定任务范例的优秀少样本学习器。值得注意的是，思路链 (CoT) 提示是一种近期通过逐步解答示例来引出复杂多步骤推理的技术，它在算术和符号推理方面取得了最佳表现，而这些任务是一些高难度的 System-2 任务，不遵循 LLM 的标准扩展规律。虽然这些成功通常归因于 LLM 的少样本学习能力，但我们只需在每个答案前添加“让我们逐步思考”，就能证明 LLM 是优秀的零样本推理器。实验结果表明，我们的 Zero-shot-CoT 使用相同的单一提示模板，在各种基准推理任务上均显著优于零样本 LLM，包括算术（MultiArith、GSM8K、AQUA-RAT、SVAMP）、符号推理（Last Letter、Coin Flip）和其他逻辑推理任务（日期理解、跟踪混洗对象），无需任何手工制作的少量样本示例，例如，使用大型 InstructGPT 模型（text-davinci-002）将 MultiArith 的准确率从 17.7% 提高到 78.7%，将 GSM8K 的准确率从 10.4% 提高到 40.7%，并且与另一个现成的大型模型 540B 参数 PaLM 也实现了类似幅度的改进。这一单一提示在各种推理任务中的多功能性，暗示了LLM中尚未开发和研究不足的基本零样本能力，这表明，通过简单的提示，即可提取高级、多任务的广泛认知能力。我们希望我们的工作不仅能成为具有挑战性的推理基准的最小最强零样本基线，还能凸显在构建微调数据集或少样本样本之前，仔细探索和分析LLM中隐藏的海量零样本知识的重要性。
