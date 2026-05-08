# Story Generation Plan

## Purpose

Requirements Analysisで定義したサービスIntentを、ユーザー中心のストーリー、受け入れ基準、ペルソナへ変換する。

このステージでは、普通のタスク管理アプリに見えないよう、以下の心理と体験を明確に扱う。

- 先延ばし
- 罪悪感の軽減
- 判断放棄
- アプリへの責任委譲
- 言い訳
- 反省回避
- サボる時間の正当化

## Story Development Approach

推奨アプローチは **Hybrid: User Journey-Based + Capability-Based** とする。

- **User Journey-Based**: 入力、推定、サボり時間確認、危険圏確認、失敗時の言い訳までの体験を自然な順序で整理する。
- **Capability-Based**: Unit分解へ接続できるよう、「人をダメにする能力」単位も併記する。

## Alternative Breakdown Options

### Option A: User Journey-Based
- **Benefit**: MVPデモの流れと一致し、ユーザー体験を説明しやすい。
- **Trade-off**: Unit分解時に技術・能力単位へ再整理が必要。

### Option B: Feature-Based
- **Benefit**: 実装機能へ直接接続しやすい。
- **Trade-off**: 普通のCRUD/タスク管理アプリに見えやすい。

### Option C: Persona-Based
- **Benefit**: 先延ばしタイプごとの心理が見えやすい。
- **Trade-off**: MVPで必要な実装範囲が膨らみやすい。

### Option D: Capability-Based
- **Benefit**: 「人をダメにする能力」とビジネス能力をUnit分解に接続しやすい。
- **Trade-off**: 実際のユーザージャーニーが見えにくくなる可能性がある。

### Option E: Hybrid
- **Benefit**: ユーザー体験、審査説明、Unit分解を同時に満たしやすい。
- **Trade-off**: ストーリー文書の構成管理に注意が必要。

## Planning Questions

以下の質問に回答してください。すべての `[Answer]:` を埋めた後、計画を承認するか修正を依頼してください。

## Question 1
User Storiesのストーリー分解アプローチはどれにしますか？

A) User Journey-Based: 入力から結果表示、言い訳生成までの流れを中心にする
B) Capability-Based: 人をダメにする能力単位を中心にする
C) Persona-Based: 先延ばしタイプ別の心理を中心にする
D) Hybrid: User Journey-Based + Capability-Basedを併用する
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 2
ペルソナはどの粒度で作成しますか？

A) MVP向けに2名だけ作成する
B) 標準的に3名作成する
C) 心理タイプを広げて4名以上作成する
D) 主要ペルソナ1名と補助ペルソナ2名に分ける
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 3
最重要ペルソナとして扱う先延ばしタイプはどれですか？

A) 罪悪感は強いが、始める判断を先送りする人
B) 見積もりが甘く、毎回ギリギリで焦る人
C) 言い訳を先に考えて安心したい人
D) サボる時間を正当化して楽しみたい人
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 4
User Storiesで最も強調したい「人をダメにする効果」はどれですか？

A) 自分で着手判断しなくてよくなる
B) サボる罪悪感が減る
C) 失敗しても言い訳で心理的に逃げられる
D) 危険圏でもアプリに責任を預けられる
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 5
ストーリーの受け入れ基準はどの形式にしますか？

A) 箇条書きの確認条件
B) Given/When/Then形式
C) 箇条書き + 主要ストーリーのみGiven/When/Then
D) デモ観点と実装観点を分けて記述する
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 6
言い訳生成のストーリーはどの位置づけにしますか？

A) MVP必須ストーリーとして扱う
B) 危険圏または失敗時の補助ストーリーとして扱う
C) デモ映え用ストーリーとして扱う
D) 後続拡張ストーリーとして扱う
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 7
LLM推定に関するストーリーはどの重点で扱いますか？

A) タスク内容から作業時間を推定する体験を中心にする
B) LLM失敗時にもフォールバックで破綻しない体験を中心にする
C) LLM推定とルール推定の根拠比較を中心にする
D) MVPではユーザー体験として見せ、技術詳細はAcceptance Criteriaに留める
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 8
ストーリー優先度はどの分類で記述しますか？

A) MVP / Later の2段階
B) Must / Should / Could の3段階
C) Demo Critical / MVP Core / Later の3段階
D) 優先度は付けず、User Journey順に並べる
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 9
User StoriesとUnit分解の接続はどのように表現しますか？

A) 各ストーリーに関連Capabilityを記載する
B) 各ストーリーに想定Unit候補を記載する
C) ストーリーとCapability/Unit候補の対応表を作成する
D) User Storiesでは対応表を作らず、Units Generationで整理する
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 10
ストーリー文書のトーンはどれにしますか？

A) 実用性中心で、テーマ表現は控えめにする
B) テーマ性を強く出し、サボりや判断放棄を明確に書く
C) 審査資料向けに攻めた表現、実装向けに補足を併記する
D) ユーザーを責めないユーモア中心にする
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Execution Checklist

- [x] Step 1: Load approved requirements from `aidlc-docs/inception/requirements/requirements.md`
- [x] Step 2: Validate all planning answers are complete and unambiguous
- [x] Step 3: Define personas in `aidlc-docs/inception/user-stories/personas.md`
- [x] Step 4: Generate user stories in `aidlc-docs/inception/user-stories/stories.md`
- [x] Step 5: Ensure all stories follow INVEST criteria
- [x] Step 6: Add acceptance criteria to each user story
- [x] Step 7: Map personas to relevant user stories
- [x] Step 8: Map stories to capabilities or Unit candidates according to the approved approach
- [x] Step 9: Add theme-fit notes explaining how each major story makes the user worse
- [x] Step 10: Add business value notes explaining why the story is still useful
- [x] Step 11: Validate generated Markdown content before saving
- [x] Step 12: Update this plan checklist immediately after each completed step
- [x] Step 13: Update `aidlc-docs/aidlc-state.md`
- [x] Step 14: Log completion and request user approval

## Mandatory Artifacts

- [x] `aidlc-docs/inception/user-stories/stories.md`
- [x] `aidlc-docs/inception/user-stories/personas.md`

## INVEST Compliance Plan

Each story will be checked for:

- **Independent**: It can be understood without requiring another story.
- **Negotiable**: It states user value, not a fixed implementation detail.
- **Valuable**: It provides user value, business value, or theme value.
- **Estimable**: It is specific enough to size later.
- **Small**: It can fit into MVP or a clear later increment.
- **Testable**: It has acceptance criteria.

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | User Stories planning does not produce design, code, or test artifacts subject to PBT enforcement. |
