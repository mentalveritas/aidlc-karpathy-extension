# aidlc-karpathy-extension

[English](README.md) | [한국어](README.ko.md) | [简体中文](README.zh-CN.md)

AI-DLC拡張機能。コード生成フェーズにおいて、Andrej Karpathyのコーディング原則（シンプルさ、最小限の変更、目標駆動実行）をブロッキング制約として適用します。

## これは何ですか？

この拡張機能は2つのアプローチを組み合わせたものです：

- **[AI-DLC](https://github.com/awslabs/aidlc-workflows)** -- AI支援ソフトウェア開発のための構造化された適応型ワークフロー方法論
- **[Andrej Karpathyのコーディング原則](https://github.com/forrestchang/andrej-karpathy-skills)** -- LLMコーディングにおける一般的な失敗パターンを行動ルールとして体系化したもの

成果物は10個のブロッキングルール（KARPATHY-01からKARPATHY-10）で、AI-DLCワークフローにオプトイン拡張として統合され、Constructionフェーズでコード品質規律を強制します。

## ルール概要

| ルール | タイトル | 原則 |
|---|---|---|
| KARPATHY-01 | 仮定の明示 | コーディング前に考える |
| KARPATHY-02 | 複雑さへの反論 | コーディング前に考える + シンプルさ |
| KARPATHY-03 | 投機的機能の禁止 | シンプルさ優先 |
| KARPATHY-04 | 最小限の抽象化 | シンプルさ優先 |
| KARPATHY-05 | 必要な変更のみ | 外科的変更 |
| KARPATHY-06 | ブラウンフィールドのスコープ規律 | 外科的変更 |
| KARPATHY-07 | 目標駆動検証 | 目標駆動実行 |
| KARPATHY-08 | 反復的収束 | 目標駆動実行 |
| KARPATHY-09 | テストファーストのバグ修正 | 目標駆動実行 |
| KARPATHY-10 | 測定可能な完了 | 目標駆動実行 |

## インストール

`extensions/coding/karpathy-principles/` ディレクトリをAI-DLCルール構造にコピーしてください：

```
aidlc-rules/
└── aws-aidlc-rule-details/
    └── extensions/
        ├── coding/
        │   └── karpathy-principles/
        │       ├── karpathy-principles.md          # 完全なルール（オプトイン時にロード）
        │       └── karpathy-principles.opt-in.md   # オプトインプロンプト
        ├── security/
        │   └── baseline/
        └── testing/
            └── property-based/
```

`core-workflow.md`の修正は不要です。AI-DLCはワークフロー開始時に`extensions/`ディレクトリを再帰的にスキャンして自動的に検出します。

## 動作の仕組み

1. ワークフロー開始時、AI-DLCが`extensions/`をスキャンし`karpathy-principles.opt-in.md`をロード
2. 要件分析フェーズでユーザーにこの拡張機能の有効化を確認
3. 有効化されると`karpathy-principles.md`がロードされ、10個のルールがブロッキング制約となる
4. 以降の各適用可能なフェーズで、進行前にコンプライアンスを検証

## オプトインオプション

- **A) Yes** -- すべてのルールを適用（ブラウンフィールド/メンテナンスに推奨）
- **B) Partial** -- シンプルさ＋外科的変更ルールのみ、目標駆動実行はスキップ
- **C) No** -- すべてのルールをスキップ（ラピッドプロトタイピング用）

## クレジット

- [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) (MIT-0 License)
- [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills) (MIT License)

## ライセンス

MIT
