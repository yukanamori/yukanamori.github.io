---
title: "AWS AI-DLC（AI駆動開発ライフサイクル）とは？ ── AI時代のソフトウェア開発手法を解説"
description: "AWSが提唱するAI-Driven Development Lifecycle（AI-DLC）の概要を、3つのフェーズ・主要メカニズム・日本での展開状況とともに解説します。"
pubDate: 2026-03-29
---

GitHub Copilot、Amazon Q Developer、Cursor、Claude Code――AIコーディングアシスタントはもはや珍しいものではなくなりました。しかし、多くのチームがこれらのツールを「既存の開発プロセスに後付けで組み込んでいる」のが現状ではないでしょうか。

もし開発ライフサイクル全体を、最初からAIを前提に再設計したらどうなるか？　その問いに対するAWSの回答が **AI-DLC（AI-Driven Development Lifecycle）** です。

2025年7月にAWS DevOps blogで発表されたこの方法論は、その後OSSとしてワークフローが公開され、日本でも「Unicorn Gym」と呼ばれる体験ワークショップが次々と開催されるなど、急速に広がりを見せています。本記事ではAI-DLCの全体像を紹介します。

## AI-DLCとは

AI-DLCは「**AI-Driven Development Lifecycle**（AI駆動開発ライフサイクル）」の略称で、AWSが提唱するソフトウェア開発方法論です。2025年7月31日にAWS DevOps & Developer Productivity Blogで公開され、同年8月8日には日本語翻訳も公開されました。

従来のSDLC（Software Development Lifecycle）との根本的な違いは、AIの位置づけにあります。

- **従来**: 人間が作業し、AIが補助する
- **AI-DLC**: AIが実行し、人間が指導・検証する

つまり、AIをコード補完ツールとしてではなく、開発ライフサイクル全体を通じた「アクティブな協力者」として位置づけるのがAI-DLCの核心です。この方法論は「freely accessible methodology」として無償で公開されており、特定のツールに縛られない汎用的なフレームワークとして設計されています。

## 3つのフェーズ

AI-DLCは大きく3つのフェーズで構成されています。

### Inception Phase（着想フェーズ）

ビジネス上の意図やハイレベルな要件を、AIと人間が協働して詳細な仕様に練り上げるフェーズです。

中心となるのは **Mob Elaboration（モブ精緻化）** と呼ばれる儀式です。クロスファンクショナルなチームとAIエージェントが集まり、AIが生成した詳細要件・ストーリー・ユニットに対して、チームがリアルタイムでフィードバックを返します。

成果物としてはユーザーストーリー、受入基準、アーキテクチャ決定記録などが生成されます。従来は複数クォーターかかることもあった要件定義が、4時間から1日程度に短縮されるケースが報告されています。

### Construction Phase（構築フェーズ）

AIがコードを生成し、人間がレビュー・検証するフェーズです。**Mob Construction（モブ構築）** の儀式のもと、以下の **Plan-Verify-Generateサイクル** を反復します。

1. **Plan** ── AIが論理アーキテクチャ、ドメインモデル、実装アプローチを提案
2. **Verify** ── 人間（またはテスト）が計画を検証
3. **Generate** ── AIがコード・テストを生成
4. **Verify** ── 人間が生成物をレビュー

このサイクルにおいて重要なのは、開発者が生成されたすべてのコード行を理解することです。AIに丸投げするのではなく、人間がコンテキストと判断を維持し続ける設計になっています。

### Operations Phase（運用フェーズ）

デプロイメントとインフラ管理のフェーズです。前のフェーズで蓄積されたコンテキスト（要件、設計判断、コード構造）をAIがそのまま活用し、Infrastructure as Code（IaC）の生成、CI/CDパイプラインの構成、モニタリングのセットアップなどを支援します。

## 3つの柱と9ステージ

AI-DLCが単なるTips集ではなく「方法論」として成立している背景には、**3つの構造的な柱** があります。

- **Artefacts（成果物）**: 各ステージが生成・消費する成果物が定義され、トレーサビリティの連鎖を形成
- **Phases（フェーズ）**: 上述の3フェーズによる時間的構造
- **Roles and Rituals（役割と儀式）**: Mob Elaboration、Mob Construction、レビューゲートなど、人間とAIの相互作用を定義

また、3つのフェーズを横断する **9つのステージ** が定義されており、要件定義からコード生成まで、セマンティックコンテキスト（意味的文脈）を段階的に充実させていくフレームワークになっています。

さらに **Adaptive Workflows（適応型ワークフロー）** という仕組みにより、タスクの種類に応じて適用するステージが動的に選択されます。例えば、バグ修正は新機能開発よりも少ないステージで処理されます。9つの各ステージの詳細については、別の記事で深掘りする予定です。

## オープンソースと実装ツール

AI-DLCは概念の紹介にとどまらず、実装可能なワークフローとしてもオープンソースで公開されています。

2025年11月29日、AWSはAI-DLCのAdaptive WorkflowsをOSSとして公開しました。これはAmazon Q DeveloperやKiroのRules/Steeringとして実装されており、メインリポジトリは [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows) です。2026年3月時点でv0.1.6まで継続的にリリースされており、CodeBuildワークフローの追加など機能拡充が進んでいます。

実際の使い方としては、Amazon Q DeveloperのProject Rulesとして `.amazonq/rules` ディレクトリにMarkdownファイルを配置する形式です。

AI-DLCの進化を時系列で見ると、以下のように整理できます。

- **2025年7月**: 概念・方法論としての紹介（AWS DevOps blog）
- **2025年11月**: 実装可能なワークフローのOSS公開
- **2026年1月〜3月**: 継続的な改善リリース（v0.1.0 → v0.1.6）

概念から実装へ、そして実装の継続的改善へと着実にステップを進めていることがわかります。

## 日本での展開: Unicorn Gym

日本ではAI-DLCを短期集中で体験・実践する **AI-DLC Unicorn Gym** というワークショップが展開されており、AWS Japan公式ブログで複数の事例レポートが公開されています。

- **2025年9月** ── 東京海上日動（金融業界初の事例）
- **2025年11月** ── LIFULL（不動産領域）
- **2026年1月** ── 弥生（会計ソフト）
- **2026年2月** ── 11社合同開催（AWS Loft Tokyo）
- **2026年3月** ── 三菱電機（製造業）、タイミー（スタートアップ）

金融、不動産、製造業、スタートアップと、業界を問わず幅広い企業がAI-DLCの実践に取り組んでいることがわかります。またSCSKなどのSIerもAI-DLC活用推進プロジェクトを公表しており、開発者カンファレンス「Developers Summit 2026」でもAI-DLC入門セッションが実施されるなど、国内での認知が急速に広がっています。

## 報告されている実績

AI-DLCを導入した組織からは、以下のような成果が報告されています。

- **10〜15倍** の生産性向上（Wipro、Dun & Bradstreetの事例）
- **80%以上** のスプリントコミットメント達成率
- Wiproでは **20時間で4つの本番レベルモジュール** を構築

ただし、これらは早期導入者の成果であり、すべての組織・プロジェクトで同様の結果が得られるとは限らない点は留意が必要です。

## まとめ

AI-DLCは、AIを単なるコード補完ツールではなく、ソフトウェア開発ライフサイクル全体の協力者として位置づける、AWSが提唱する構造化されたアプローチです。

3つのフェーズ、9つのステージ、Adaptive Workflowsという体系的なフレームワークを提供し、OSSとしてワークフローが公開されていることで、概念にとどまらず実践可能な方法論となっています。日本でもUnicorn Gymを通じて多くの企業が導入を進めており、今後ますます普及が進むと考えられます。

次回以降の記事では、各フェーズの詳細な解説や、Amazon Q Developerを使ったハンズオンなどを取り上げていく予定です。

## 参考リンク

### 公式ブログ（英語）

- [AI-Driven Development Life Cycle: Reimagining Software Engineering](https://aws.amazon.com/blogs/devops/ai-driven-development-life-cycle/)
- [Open-Sourcing Adaptive Workflows for AI-DLC](https://aws.amazon.com/blogs/devops/open-sourcing-adaptive-workflows-for-ai-driven-development-life-cycle-ai-dlc/)
- [Building with AI-DLC using Amazon Q Developer](https://aws.amazon.com/blogs/devops/building-with-ai-dlc-using-amazon-q-developer/)

### 公式ブログ（日本語）

- [AI 駆動開発ライフサイクル: ソフトウェアエンジニアリングの再構築](https://aws.amazon.com/jp/blogs/news/ai-driven-development-life-cycle/)

### ドキュメント

- [AWS Prescriptive Guidance: Accelerating Software Development Lifecycles with Generative AI](https://docs.aws.amazon.com/prescriptive-guidance/latest/strategy-accelerate-software-dev-lifecycle-gen-ai/introduction.html)

### GitHub

- [awslabs/aidlc-workflows](https://github.com/awslabs/aidlc-workflows)

### Unicorn Gym 事例（日本語）

- [金融業界初 AI-DLC Unicorn Gym による開発変革への挑戦](https://aws.amazon.com/jp/blogs/news/tokio-marine-ai-dlc/)
- [11 社合同 AI-DLC Unicorn Gym で体験した開発のパラダイムシフト](https://aws.amazon.com/jp/blogs/news/joint-ai-dlc-unicorn-gym-202601/)
