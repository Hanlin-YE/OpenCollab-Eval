# 将 SOL 反馈写入自我改进记忆

[English](../sim-feedback.md) | **简体中文**

收集官方终评后，在可信的本地 SOL 实验目录上执行回放。

```bash
oc-eval sim-feedback ./campaign > feedback.json
jq '.memory' feedback.json > next-memory.json
```

实验目录包含任务清单、由哈希绑定的版本 1 源记忆，以及位于
`cases/NNN/{before,after}/submission.py` 的冻结源码。
`cases/NNN/after/prediction.json` 中的修订预测记录父子源码和输入记忆的身份。
每个版本在 `deepseek-official/NNN/{before,after}/` 下保存已核验的提交回执和终评。
可选的 `deepseek-b200-snapshot.json` 按提交号索引已收集的终评。调用方负责核对回执和结果的真实性。

每组完成配对的提交将预测及实际结果写入优化器历史，失败修订也进入记录。
存在匹配源码的 A800 评测时，历史同时记录本地通过的工作负载数。
选版采用 Private B200 v1.1 终评正确且 SOL 更高的源码。平分保留初版；
终评待出时保留已经通过的初版。空值表示当前尚无确认可用的父版。
输出中的源码路径相对于实验目录。命令读取本地证据，将 JSON 写到标准输出。
