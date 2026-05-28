# Agent Governance Toolkit かんたん解説（日本語）

このファイルは、`README.md` の内容を「まず全体像を掴みたい人向け」に整理した入門ガイドです。

## これは何？

**Agent Governance Toolkit（AGT）** は、AIエージェントの実行時ガバナンス基盤です。  
ポイントは「モデルにお願いする安全策」ではなく、**アプリケーション側でエージェントの行動を強制的に制御する**ことです。

AGTは主に次を提供します。

- ポリシー適用（許可・拒否・承認フロー）
- エージェント識別と信頼管理
- 監査ログ（誰が何をしようとして、なぜ許可/拒否されたか）
- セキュリティ/コンプライアンス検証

## どんな課題を解決する？

AIエージェント運用でよくある以下の問題に対応します。

1. **この操作は許可してよいか？**
   - 例: `drop_table` のような破壊的操作を禁止したい
2. **誰（どのエージェント）が実行したか追跡できるか？**
3. **監査で証明できる記録が残るか？**

## まずは最短で使う（Python）

前提: Python 3.10+

```bash
pip install agent-governance-toolkit[full]
```

最小イメージ:

1. `policy.yaml` を作る（許可/拒否ルール）
2. ツール関数を `govern(...)` でラップする
3. 呼び出しごとにポリシー判定とログ記録が走る

詳細は公式クイックスタート: [`/docs/quickstart.md`](/docs/quickstart.md)

## まず作るべき policy.yaml の考え方

最初はシンプルで十分です。

- 「危険操作は deny」
- 「外部送信は require_approval」
- 「それ以外は allow（または fail-closed設計）」

運用しながら段階的に厳しくしていくのが実践的です。

## CLIでできること（運用・CI向け）

- `agt doctor` : インストール/環境確認
- `agt verify` : コンプライアンス関連の検証
- `agt red-team scan ...` : プロンプト攻撃耐性の点検
- `agt lint-policy ...` : ポリシーファイルの静的検証

CIで使う場合は `agt verify --strict` のように厳格モードが使えます。

## どのパッケージを選ぶべき？

- **まず試す**: Python版（機能が最も広い）
- **既存スタックに合わせる**: TypeScript / .NET / Rust / Go SDK
- **Copilot CLI / Claude Code**: 専用パッケージあり

対応パッケージ一覧は [`/README.md`](/README.md) の Install セクションと [`/docs/PACKAGE-FEATURE-MATRIX.md`](/docs/PACKAGE-FEATURE-MATRIX.md) を参照してください。

## 導入のおすすめ順

1. 重要ツールだけポリシーでガード
2. 監査ログを有効化して可視化
3. 承認フロー（require_approval）を追加
4. 組織要件に合わせてコンプライアンス検証をCIへ組み込み

## よくある誤解

- AGTは「LLMの出力を完全に無害化する製品」ではありません。
- AGTの主眼は、**エージェントの実行アクションを制御・監査可能にすること**です。

## 次に読むと理解しやすい順番

1. [`/docs/quickstart.md`](/docs/quickstart.md)
2. [`/examples/quickstart/`](/examples/quickstart/)
3. [`/docs/ARCHITECTURE.md`](/docs/ARCHITECTURE.md)
4. [`/docs/tutorials/`](/docs/tutorials/)
5. [`/docs/compliance/owasp-agentic-top10-architecture.md`](/docs/compliance/owasp-agentic-top10-architecture.md)

## 補足

- 公式の包括情報は `README.md`（英語）が最新です。
- このファイルは「概要把握」に特化した要約版です。
