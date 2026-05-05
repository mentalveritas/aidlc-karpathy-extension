# aidlc-karpathy-extension

[English](README.md) | [한국어](README.ko.md) | [日本語](README.ja.md)

AI-DLC 扩展，在代码生成阶段将 Andrej Karpathy 的编码原则（简洁性、最小变更、目标驱动执行）作为阻断性约束强制执行。

## 这是什么？

本扩展结合了两种方法：

- **[AI-DLC](https://github.com/awslabs/aidlc-workflows)** -- 用于 AI 辅助软件开发的结构化自适应工作流方法论
- **[Andrej Karpathy 的编码原则](https://github.com/forrestchang/andrej-karpathy-skills)** -- 将 LLM 编码常见失败模式系统化为行为规则

最终产出为 10 条阻断规则（KARPATHY-01 至 KARPATHY-10），作为可选扩展集成到 AI-DLC 工作流中，在 Construction 阶段强制执行代码质量规范。

## 规则概览

| 规则 | 标题 | 原则 |
|---|---|---|
| KARPATHY-01 | 明确假设 | 编码前先思考 |
| KARPATHY-02 | 质疑复杂性 | 编码前先思考 + 简洁性 |
| KARPATHY-03 | 禁止投机性功能 | 简洁优先 |
| KARPATHY-04 | 最小抽象 | 简洁优先 |
| KARPATHY-05 | 仅做必要变更 | 精准变更 |
| KARPATHY-06 | 棕地项目范围约束 | 精准变更 |
| KARPATHY-07 | 目标驱动验证 | 目标驱动执行 |
| KARPATHY-08 | 迭代收敛 | 目标驱动执行 |
| KARPATHY-09 | 测试先行修复 | 目标驱动执行 |
| KARPATHY-10 | 可量化完成 | 目标驱动执行 |

## 安装

将 `extensions/coding/karpathy-principles/` 目录复制到您的 AI-DLC 规则结构中：

```
aidlc-rules/
└── aws-aidlc-rule-details/
    └── extensions/
        ├── coding/
        │   └── karpathy-principles/
        │       ├── karpathy-principles.md          # 完整规则（选择启用后加载）
        │       └── karpathy-principles.opt-in.md   # 选择启用提示
        ├── security/
        │   └── baseline/
        └── testing/
            └── property-based/
```

无需修改 `core-workflow.md`。AI-DLC 在工作流启动时会递归扫描 `extensions/` 目录自动发现扩展。

## 工作原理

1. 工作流启动时，AI-DLC 扫描 `extensions/` 并加载 `karpathy-principles.opt-in.md`
2. 在需求分析阶段，询问用户是否启用此扩展
3. 启用后加载 `karpathy-principles.md`，10 条规则成为阻断性约束
4. 在后续每个适用阶段，验证合规性后才允许继续

## 选择启用选项

- **A) Yes** -- 强制执行所有规则（推荐用于棕地项目/维护工作）
- **B) Partial** -- 仅执行简洁性和精准变更规则，跳过目标驱动执行
- **C) No** -- 跳过所有规则（适用于快速原型开发）

## 致谢

- [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) (MIT-0 License)
- [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) (MIT License)

## 许可证

MIT
