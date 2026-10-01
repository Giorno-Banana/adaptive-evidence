# Adaptive Evidence — 第二期文本赛道候选

参赛方向：Agent Memory Challenge 第二期，开源方法榜、文本记忆赛道。版本 `0.2.0`。

作者与维护者：王子铭（[Giorno-Banana](https://github.com/Giorno-Banana)）。

源码：[Giorno-Banana/adaptive-evidence](https://github.com/Giorno-Banana/adaptive-evidence)。本项目新代码采用 [MIT 许可证](LICENSE)，依赖、模型和诊断数据保留各自许可。

系统通过 Add 持久化原始记忆，通过 Search 返回有来源的原文片段。检索使用 `text-embedding-v4`、BM25、RRF 和相邻原文窗口。最终 Answer/Eval 由 AML 统一执行。本版本没有官方成绩。

默认配置通过百炼 DashScope 原生 HTTP 接口调用 `text-embedding-v4`，维度 1024；可选检索规划器固定 `gpt-4o-mini`，默认关闭。关闭规划器仍会产生 Embedding 调用。BGE 仅保留为显式选择的历史实验配置，不是本次拟提交配置。

## 接入步骤

1. 安装 `requirements.txt`，将 `env.example` 复制为私有 `.env`，填入服务鉴权密钥、百炼密钥和该账户对应的原生 HTTP endpoint。
2. 运行 `python preflight.py --env-file .env`，先做不调用模型的配置检查。
3. 运行 `python preflight.py --env-file .env --live`，对少量合成记忆做真实模型调用验证。
4. 按 [运行说明](OPERATIONS.md) 部署，再填好 [提交材料](SUBMISSION.md)。
5. 公开源码并固定 commit，向 AML 申请 Key，通过文本赛道官方 Smoke 后再提交 Full。

实际完成情况与外部依赖见 [第二期状态](CYCLE2_STATUS.md)，方法来源见 [PROVENANCE.md](PROVENANCE.md)。`preflight --live` 是自测，不会启动官方 Smoke 或 Full。

2026-10-01 已完成真实百炼接入、40 题公开集检索诊断与本机 HTTP/进程重启检查，详见 [小样本报告](PILOT_RESULTS.md)。该结果不含答案评分，不是官方成绩。

## 验证

```bash
python -m unittest discover -s tests -v
```

发布包只包含服务与其相关测试，不包含历史 QA 模型、数据集、评测答案、模型权重、数据库或真实凭证。测试中的向量服务是模拟服务，不能用于声称真实模型效果。

## 规则与 API 依据

- [赛事与 FAQ](https://agentmemories.ai/competition/)
- [AML API 契约](https://agentmemories.ai/api-guide)
- [百炼文本向量同步接口](https://help.aliyun.com/zh/model-studio/text-embedding-synchronous-api/)

2026-09-29 核验：赛事 FAQ 的学术榜模型要求是 `text-embedding-v4` 与 `gpt-4o-mini`，通用文档措辞不完全一致。本候选按 FAQ 准备，最终资格由赛事方核验。
