# Unit of Work Plan

## Purpose

Requirements、User Stories、Application Designをもとに、MVP実装へ進めるためのUnit of Workを生成する。

このプロジェクトでは、審査基準の「Unit分解の適切さ」が重要である。したがって、Unitは単なるUI/API/DBの技術単位ではなく、以下を併記する。

- 人をダメにする能力
- ビジネス能力
- 実装対象となる画面/API/データ
- 対応するUser Stories
- MVPに含めるか、後続拡張に回すか

## Proposed Decomposition Direction

初期候補は以下とする。

1. Deadline Decision Unit
2. Workload Estimation Unit
3. Idle Planning Unit
4. Excuse Generation Unit
5. History and Persistence Unit
6. Demo Experience Unit

`Idle Entertainment Recommender` はMVPでは `Idle Planning Unit` に含め、後続で `Idle Entertainment Recommendation Unit` として独立できる扱いにする。

## Planning Questions

以下の質問に回答してください。すべての `[Answer]:` を埋めた後、計画を承認するか修正を依頼してください。

## Question 1
Unit分解の主軸はどれにしますか？

A) Capability-first: 人をダメにする能力単位を主軸にする
B) Implementation-first: 実装しやすい技術境界を主軸にする
C) Hybrid: Capability単位を主軸にし、各Unit内に画面/API/データを併記する
D) Demo-flow-first: MVPデモの画面遷移順を主軸にする
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 2
MVPのUnit数はどの程度にしますか？

A) 3から4 Unitに圧縮する
B) 5から6 Unitで主要能力ごとに分ける
C) 7 Unit以上で細かく分ける
D) MVP UnitとLater Unitを分け、MVPは5から6 Unitにする
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 3
`Idle Entertainment Recommender` はUnits Generationでどう扱いますか？

A) `Idle Planning Unit` の一部にする
B) `Idle Entertainment Recommendation Unit` として独立Unitにする
C) MVPでは `Idle Planning Unit` に含め、Laterで独立Unit候補として明記する
D) `LLM Assistance Unit` の一部にする
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 4
`LLM Adapter` はどのUnitに含めますか？

A) `Workload Estimation Unit` に含め、言い訳生成からも利用する
B) `Excuse Generation Unit` に含め、作業時間推定からも利用する
C) `LLM Assistance Unit` として独立Unitにする
D) MVPでは `Workload Estimation Unit` に含め、共通Adapterとして後続で独立可能にする
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 5
`Procrastination BFF API` と `Demo Experience Orchestrator` はどのUnitにしますか？

A) `Demo Experience Unit` として独立させる
B) 各UnitのAPIとして分散させる
C) フロントエンドUnitに含める
D) MVPでは独立Unitにせず、Constructionで実装都合に合わせる
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 6
履歴と永続化はどのUnitにしますか？

A) `History and Persistence Unit` としてまとめる
B) `Self-Deception History Unit` と `Persistence Unit` に分ける
C) 各Unitが必要な履歴を直接持つ
D) MVPではRepository中心にし、履歴分析はLaterに回す
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 7
Unit間依存の基本方針はどれにしますか？

A) Demo Experience Unitが各Unitを呼び出す
B) Deadline Decision Unitを中心に各Unitを呼び出す
C) 各Unitをできるだけ独立させ、BFF/API層で統合する
D) Event-driven風にして後続で非同期化しやすくする
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 8
コード組織戦略はどれにしますか？

A) 単一Webアプリ内のモジュールとしてUnitを分ける
B) フロントエンド、API、DBを分け、その中でUnitモジュールを作る
C) モノレポで `apps/web`, `apps/api`, `packages/domain` のように分ける
D) AWSサーバーレス前提でLambda単位に分ける
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 9
PBT対象Unitをどこに明記しますか？

A) Deadline Decision Unitのみ
B) Deadline Decision UnitとWorkload Estimation Unit
C) Bに加えてIdle Planning Unit
D) Bに加えてIdle Planning UnitとHistory and Persistence Unit
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 10
Unit成果物で審査向けに最も強調したい観点はどれですか？

A) Unitごとの人をダメにする効果
B) Unitごとのビジネス価値
C) UnitごとのUser Story対応
D) A、B、Cに加えて画面/API/データの実装対象
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Execution Checklist

- [x] Step 1: Load approved requirements, user stories, application design, and execution plan
- [x] Step 2: Validate all Unit planning answers are complete and unambiguous
- [x] Step 3: Define unit decomposition approach and code organization strategy
- [x] Step 4: Generate `unit-of-work.md` with unit definitions, responsibilities, human-degrading effect, business value, and code organization strategy
- [x] Step 5: Generate `unit-of-work-dependency.md` with dependency matrix and sequencing
- [x] Step 6: Generate `unit-of-work-story-map.md` mapping stories to units
- [x] Step 7: Ensure every story is assigned to at least one unit
- [x] Step 8: Ensure every MVP component maps to a unit
- [x] Step 9: Validate Markdown content before saving
- [x] Step 10: Update this plan checklist immediately after each completed step
- [x] Step 11: Update `aidlc-docs/aidlc-state.md`
- [x] Step 12: Log completion and request user approval

## Mandatory Artifacts

- [x] `aidlc-docs/inception/application-design/unit-of-work.md`
- [x] `aidlc-docs/inception/application-design/unit-of-work-dependency.md`
- [x] `aidlc-docs/inception/application-design/unit-of-work-story-map.md`

## Unit Quality Checklist

- [x] Each Unit has a clear responsibility
- [x] Each Unit maps to User Stories
- [x] Each Unit explains how it makes the user worse
- [x] Each Unit explains user value and business value
- [x] Each Unit lists screens, APIs, and data targets
- [x] MVP vs Later scope is clear
- [x] Dependencies are explicit
- [x] PBT candidates are carried forward

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | Units Generation maps PBT candidate areas to units. Enforced PBT rules apply in Functional Design, NFR Requirements, Code Generation, and Build/Test. |
