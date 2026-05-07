# Application Design Plan

## Purpose

Requirements、User Stories、Workflow Planningで定義された内容をもとに、MVP実装へ接続できる高レベルのアプリケーション設計を作成する。

この設計では、普通のCRUD構成に見えないよう、以下の独自コンポーネントを中心に据える。

- Procrastination Deadline Engine
- LLM Workload Estimator
- Guilt-Free Idle Planner
- Excuse Generator
- Self-Deception History
- Demo Experience Orchestrator

## Design Scope

Application Designでは、詳細なビジネスルールやアルゴリズムの式までは確定しない。以下を定義する。

- コンポーネント名
- 目的
- 責務
- 外部インターフェース
- 主要メソッドのシグネチャ
- サービス層のオーケストレーション
- コンポーネント依存関係
- データフロー

詳細な推定式、成功確率計算、PBTの具体テスト設計はConstructionのFunctional DesignとCode Generationで扱う。

## Planning Questions

以下の質問に回答してください。すべての `[Answer]:` を埋めた後、計画を承認するか修正を依頼してください。

## Question 1
Application Designで最も強調する設計スタイルはどれにしますか？

A) Capability-first: 人をダメにする能力単位を主構造にし、画面/API/DBは各能力に紐づける
B) Service-first: APIサービスやデータ永続化など、実装しやすいサービス境界を主構造にする
C) Experience-first: デモ体験の流れを主構造にし、裏側のコンポーネントを補助として定義する
D) Hybrid: Capability-firstを主軸にしつつ、MVP実装しやすいサービス境界も併記する
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 2
LLM利用コンポーネントの設計境界はどれにしますか？

A) Workload Estimator内にLLM呼び出しとフォールバックをまとめる
B) LLM Adapterを独立させ、EstimatorはLLM結果とフォールバック結果を統合する
C) MVPではLLMを抽象インターフェース化し、実装詳細はConstructionで決める
D) LLMはWorkload EstimationとExcuse Generationの共通Adapterとして設計する
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 3
Procrastination Deadline Engineの責務範囲はどこまでにしますか？

A) 最遅着手時刻、成功確率、リスク帯の算出に限定する
B) Aに加えて、推定根拠とユーザー向け説明文の生成も含める
C) Aに加えて、サボってよい時間と現実逃避プランも含める
D) Aに限定し、説明文やサボりプランは別コンポーネントへ分離する
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 4
言い訳生成はどのような設計位置づけにしますか？

A) 独立したExcuse Generatorコンポーネントにする
B) Failure Flowの一部として設計する
C) LLM Adapter配下の生成機能として扱う
D) MVPデモ用の軽量コンポーネントとして設計し、後続で拡張可能にする
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 5
履歴データの設計責務はどれにしますか？

A) Self-Deception Historyが保存・検索・類似履歴取得を担当する
B) Repository層が保存・検索を担当し、Historyはドメイン分析だけ担当する
C) MVPではTask History Repositoryとしてシンプルにし、後続でSelf-Deception Historyへ拡張する
D) 履歴は各コンポーネントが必要に応じて直接参照する
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 6
API設計の粒度はどれにしますか？

A) 単一のAnalyze Task APIで入力から推定結果まで返す
B) Task API、Estimation API、Excuse API、History APIに分ける
C) MVPはAnalyze Task API中心にし、内部コンポーネントは分離する
D) デモ体験優先でBFF的なAPIを用意し、内部サービス境界を別に定義する
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 7
画面/UXの設計境界はどの程度Application Designに含めますか？

A) 画面名と主要表示項目だけ定義する
B) 入力画面、結果画面、言い訳画面、履歴画面の責務まで定義する
C) MVPデモの画面遷移まで定義する
D) Application DesignではUI詳細を避け、Units Generationで画面/API/データを整理する
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 8
PBTを意識した設計として、どこを純粋関数化する方針にしますか？

A) 最遅着手時刻、成功確率、リスク帯判定のみ
B) Aに加えて、サボってよい時間計算とフォールバック推定
C) Aに加えて、履歴補正ロジックも含める
D) Application Designでは候補だけ示し、Functional Designで確定する
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 9
コンポーネント依存関係の基本方針はどれにしますか？

A) Orchestratorが各コンポーネントを呼び出し、コンポーネント同士の直接依存を減らす
B) Engineが中心となり、Estimator、History、Planner、Excuse Generatorを直接呼ぶ
C) API層が必要なコンポーネントを直接呼ぶ
D) イベント駆動風に設計し、後続で非同期処理へ拡張しやすくする
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 10
Application Design成果物で、審査向けに特に強調したい観点はどれですか？

A) 独自コンポーネント名と責務
B) 人をダメにする効果とビジネス価値の両立
C) MVP実装可能性とAWS拡張性
D) Unit分解へ接続しやすいCapability/Component対応
X) Other (please describe after [Answer]: tag below)

[Answer]: X BとD

## Execution Checklist

- [x] Step 1: Load approved requirements, stories, personas, and execution plan
- [x] Step 2: Validate all application design planning answers are complete and unambiguous
- [x] Step 3: Identify main functional components and their responsibilities
- [x] Step 4: Generate `components.md` with component definitions and high-level responsibilities
- [x] Step 5: Generate `component-methods.md` with method signatures, input/output types, and high-level purpose
- [x] Step 6: Generate `services.md` with service definitions and orchestration patterns
- [x] Step 7: Generate `component-dependency.md` with dependency relationships, communication patterns, and data flow
- [x] Step 8: Generate consolidated `application-design.md`
- [x] Step 9: Validate Markdown content and any diagrams before saving
- [x] Step 10: Update this plan checklist immediately after each completed step
- [x] Step 11: Update `aidlc-docs/aidlc-state.md`
- [x] Step 12: Log completion and request user approval

## Mandatory Artifacts

- [x] `aidlc-docs/inception/application-design/components.md`
- [x] `aidlc-docs/inception/application-design/component-methods.md`
- [x] `aidlc-docs/inception/application-design/services.md`
- [x] `aidlc-docs/inception/application-design/component-dependency.md`
- [x] `aidlc-docs/inception/application-design/application-design.md`

## Design Quality Checklist

- [x] Components are capability-oriented, not only CRUD-oriented
- [x] Each major component has a clear "human-degrading effect"
- [x] Each major component also has business value
- [x] LLM dependency has fallback boundaries
- [x] Pure function candidates for PBT are identified
- [x] Service orchestration is clear enough for later Functional Design
- [x] Component boundaries support Units Generation

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | Application Design identifies PBT-relevant pure function candidates, but enforced PBT rules apply later in Functional Design, NFR Requirements, Code Generation, and Build/Test. |
