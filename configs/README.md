# TOML 参数

每种运行模式的TOML参数介绍。

## 采样（Sampling）

对带有相关负对数似然 (NLL) 的 SMILES 进行采样。

| 参数               | 描述                                                         |
|--------------------|--------------------------------------------------------------|
| run_type           | 设置为 "sampling"                                            |
| use_cuda           | "true" 表示使用 GPU，"false" 表示使用 CPU                     |
| json_out_config    | TOML 文件以 JSON 格式的文件名                                 |
| [parameters]       | 开始参数部分                                                 |
| model_file         | 从中采样的模型文件的文件名                                    |
| smiles_file        | Lib/LinkInvent 和 Mol2Mol 的输入 SMILES 的文件名               |
| sample_strategy    | 仅 Mol2Mol：“束搜索”或“多项式”                               |
| output_file        | 包含示例 SMILES 和 NLLs 的 CSV 文件的文件名                    |
| num_smiles         | 需要采样的 SMILES 数量，注意：这将乘以输入 SMILES 的数量        |
| unique_molecules   | 如果“true”仅返回唯一的规范化 SMILES                           |
| randomize_smiles   | 如果“true”随机打乱输入 SMILES 中的原子                         |
| tb_logdir          | 如果不为空，则为 TensorBoard 日志记录目录的名称                |
| temperature        | 仅 Mol2Mol：默认 1.0                                         |
| target_smiles_path | 仅 Mol2Mol：如果不为空，则表示 SMILES 的文件名，检查生成提供的 SMILES 的 NLL |

## 评分（Scoring）

评分组件的接口。没有使用任何模型。

| 参数               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| run_type           | 设置为 "scoring"                                             |
| use_cuda           | "true" 表示使用 GPU，"false" 表示使用 CPU                    |
| json_out_config    | TOML 文件以 JSON 格式的文件名                                |
| [parameters]       | 开始参数部分                                                 |
| smiles_file        | SMILES 文件名，SMILES 应位于第一列                           |
| [scoring_function] | 开始评分函数设置部分                                         |
| \[[components]]     | 开始 [scoring_function] 内的组件部分，注意使用双括号以开始列表 |
| type               | "custom_sum" 表示加权算术平均，"custom_produc" 表示加权几何平均 |
| component_type     | 组件的名称，FIXME: 列出所有组件                              |
| name               | 用户选择的在 CSV 文件等中输出的名称                          |
| weight             | 此组件的权重                                                 |

## 迁移学习（Transfer Learning）

对一组输入的 SMILES 进行迁移学习。

| 参数                  | 描述                                            |
| --------------------- | ----------------------------------------------- |
| run_type              | 设置为 "transfer_learning"                      |
| use_cuda              | "true" 表示使用 GPU，"false" 表示使用 CPU       |
| json_out_config       | TOML 文件以 JSON 格式的文件名                   |
| tb_logdir             | 如果不是空字符串，为 TensorBoard 日志目录的名称 |
| [parameters]          | 开始参数部分                                    |
| num_epochs            | 运行的 epoch 数量                               |
| save_every_n_epochs   | 每 N 个 epoch 保存一次检查点文件                |
| batch_size            | 批量大小，注意：影响 SGD                        |
| num_refs              | 相似性的参考数量                                |
| sample_batch_size     | 相似性的采样数量                                |
| input_model_file      | 输入 prior 模型的文件名                         |
| smiles_file           | 用于 Lib/Linkinvent 和 Molformer 的 SMILES 文件 |
| output_model_file     | 最终模型的文件名                                |
| pairs.upper_threshold | Molformer: 相似性的上阈值                       |
| pairs.lower_threshold | Molformer: 相似性的下阈值                       |
| pairs.min_cardinality | Molformer: 最小基数                             |
| pairs.max_cardinality | Molformer: 最大基数                             |

## 阶段学习（Staged Learning）

运行强化学习 (RL) 和/或课程学习 (CL)。CL 简单来说就是多阶段的 RL 学习。

| 参数                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| run_type            | 设置为 "transfer_learning"                                   |
| use_cuda            | "true" 表示使用 GPU，"false" 表示使用 CPU                    |
| json_out_config     | TOML 文件以 JSON 格式的文件名                                |
| tb_logdir           | 如果不是空字符串，为 TensorBoard 日志目录的名称              |
| [parameters]        | 开始参数部分                                                 |
| use_checkpoint      | 如果为 "true"，则使用 agent_file 中的 diversity filter（如果存在） |
| summary_csv_prefix  | 输出 CSV 文件名的前缀                                        |
| prior_file          | prior 模型文件的文件名，用作参考                             |
| agent_file          | agent 模型文件的文件名，用于训练，当需要时用先前阶段的检查点文件替换 |
| batch_size          | 批量大小，注意：影响 SGD                                     |
| uniquify_smiles     | 如果为 "true"，则仅返回唯一的 SMILES（采样）                 |
| randomize_smiles    | 如果为 "true"，则在输入的 SMILES 中随机打乱（采样）          |
| [learning_strategy] | 开始RL 学习策略部分                                          |
| type                | 使用 "dap"                                                   |
| sigma               | 奖励函数中的 sigma                                           |
| rate                | torch 优化器的学习率                                         |
| [diversity_filter]  | 开始多样性过滤器部分                                         |
| type                | 过滤器类型的名称: "IdenticalMurckoScaffold", "IdenticalTopologicalScaffold", "ScaffoldSimilarity", "PenalizeSameSmiles" |
| bucket_size         | 在分子被评分为零之前，需要存储的scaffold数量                 |
| minscore            | 最小分数                                                     |
| minsimilarity       | "ScaffoldSimilarity" 中的最小相似性                          |
| penalty_multiplier  | "PenalizeSameSmiles" 中每个分子的惩罚                        |
| [inception]         | 开始 Inception 部分                                          |
| smiles_file         | "good" SMILES 的文件名                                       |
| memory_size         | Inception 存储的 SMILES 数量                                 |
| sample_size         | 从内存中随机采样的 SMILES 数量                               |
| \[[stage]]           | 开始一个阶段，注意双括号                                     |
| chkpt_file          | 检查点文件的文件名，将在终止和 Ctrl-C 时写入                 |
| termination         | 使用 "simple"，终止标准                                      |
| max_score           | 最大分数，用于终止                                           |
| min_steps           | 避免提前终止的最小 RL 步骤数                                 |
| max_steps           | 运行的最大 RL 步骤数，如果达到最大值，_all_ 阶段都将被终止   |

评分函数的添加方式在[评分](#评分scoring)中相同，但以 stage 为前缀。

