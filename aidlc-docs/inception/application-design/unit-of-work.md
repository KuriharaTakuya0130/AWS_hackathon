# Unit of Work

## Decomposition Strategy

Unit of Work は **Capability-first + implementation mapping** で分解する。審査向けには「人をどうダメにする能力か」を主軸に説明し、実装向けには各 Unit に画面、API、データ、PBT 対象を紐づける。

MVP は 6 Unit に収める。`Idle Entertainment Recommender` は MVP では Idle Planning Unit に含め、後続で独立 Unit に切り出せる候補として扱う。`LLM Adapter` は MVP では Workload Estimation Unit に含めるが、Excuse Generation と Idle Planning からは adapter port 経由で利用し、後続で LLM Assistance Unit に分離できる構造にする。

## Code Organization Strategy

実装時は monorepo を前提にする。

```text
apps/web
apps/api
packages/domain/deadline-decision
packages/domain/workload-estimation
packages/domain/idle-planning
packages/domain/excuse-generation
packages/domain/history
packages/domain/demo-experience
packages/shared/types
```

アプリケーションコードは workspace root に置き、`aidlc-docs/` には置かない。

## MVP Units

### UOW-001: Demo Experience Unit

**Responsibility**: MVP の一連の体験を束ねる。入力、分析、結果表示、言い訳生成、履歴記録の流れを BFF API と Orchestrator で統合する。

**Main Components**
- Procrastination BFF API
- Demo Experience Orchestrator

**User Stories**
- US-001: Minimal Task Intake
- US-002: LLM Workload Estimation
- US-003: Latest Start Decision
- US-004: Guilt-Free Idle Time
- US-005: Risk Band Explanation
- US-006: Excuse Generation
- US-007: History Recording
- US-008: Estimation Fallback

**Human-Degrading Effect**: ユーザーが細かい判断をせず、画面の流れに乗るだけで「まだサボってよい」という許可、娯楽提案、言い訳まで受け取れるようにする。

**User Value**: 最小入力で、いつまでサボれるか、どれくらい危険か、今何をして逃避できるかを短時間で確認できる。

**Business Value**: MVP デモの完成度を左右する統合 Unit。テーマ性を一画面の体験として伝えられる。

**Implementation Targets**
- Screens: Minimal Task Input, Procrastination Result, Excuse Draft, History Snapshot
- APIs: `POST /demo/analyze-task`, `POST /demo/excuse`, `POST /demo/outcome`, `GET /demo/history`
- Data: `TaskAnalysisResult`, `ExcuseDraft`, `HistoryRecord`

**MVP Scope**: Include

**Later Scope**: プレゼン用デモシナリオ、共有用 URL、認証付きユーザー別履歴。

### UOW-002: Workload Estimation Unit

**Responsibility**: タスク内容、締め切り、履歴から作業量を推定する。LLM 推定、履歴補正、フォールバック推定を統合する。

**Main Components**
- Workload Estimator
- LLM Adapter

**User Stories**
- US-002: LLM Workload Estimation
- US-008: Estimation Fallback
- US-007: History Recording, support only

**Human-Degrading Effect**: ユーザーが面倒な見積もりを自分で考えず、アプリに作業量判断を委ねられる。

**User Value**: 見積もりが苦手でも、推定作業時間、信頼度、推定根拠を得られる。LLM が使えない場合でもデモが止まらない。

**Business Value**: 外部 AI に依存しすぎない MVP を実現し、後続の精度改善や有料機能の土台になる。

**Implementation Targets**
- Screens: Procrastination Result の見積もり根拠表示
- APIs: `POST /demo/analyze-task` internal flow
- Data: `WorkloadEstimate`, `HistorySignal`, `LLMResult`

**PBT Candidate**: Yes. `fallbackEstimate`, confidence range, merge result invariants.

**MVP Scope**: Include

**Later Scope**: LLM Assistance Unit の独立、モデル選択、見積もり比較、ユーザー別補正。

### UOW-003: Deadline Decision Unit

**Responsibility**: 推定作業量と締め切りから、最も遅い着手時刻、成功確率、リスク帯、説明文を生成する。

**Main Components**
- Procrastination Deadline Engine
- Risk Explanation Composer

**User Stories**
- US-003: Latest Start Decision
- US-005: Risk Band Explanation

**Human-Degrading Effect**: 「今始めるべきか」という判断をユーザーから奪い、アプリのリスク判定に従ってサボれるようにする。

**User Value**: 断定ではなく、成功確率、安全圏、危険圏、詰みかけとして状況を理解できる。

**Business Value**: サービスの中核 Intent を担う Unit。普通のタスク管理ではない独自性を最も強く示す。

**Implementation Targets**
- Screens: Procrastination Result の latest start, success probability, risk band, uncertainty note
- APIs: `POST /demo/analyze-task` internal flow
- Data: `DeadlineDecision`, `RiskExplanation`, `BufferPolicy`

**PBT Candidate**: Yes. latest start boundary, probability range, risk band classification.

**MVP Scope**: Include

**Later Scope**: より詳細なバッファポリシー、期限超過時の failure flow 連携。

### UOW-004: Idle Planning Unit

**Responsibility**: 最新着手時刻までの「サボってよい時間」を計算し、その時間とリスク帯に応じた休憩、娯楽、現実逃避、言い訳準備プランを提案する。

**Main Components**
- Guilt-Free Idle Planner
- Idle Entertainment Recommender

**User Stories**
- US-004: Guilt-Free Idle Time
- US-005: Risk Band Explanation, support only

**Human-Degrading Effect**: サボる時間だけでなく、その時間に何をして逃避するかまでアプリに委ねられる。

**User Value**: 罪悪感を減らしながら、残り時間に合う休憩や娯楽を選べる。危険な状態では短い休憩や言い訳準備に誘導される。

**Business Value**: デモで一目で伝わるテーマ適合性を作る Unit。単なる計算結果を体験に変える。

**Implementation Targets**
- Screens: Procrastination Result の guilt-free idle time, activity suggestions
- APIs: `POST /demo/analyze-task` internal flow
- Data: `IdlePlan`, `IdleActivitySuggestion`, `IdleRecommendationContext`

**PBT Candidate**: Yes. idle time non-negative, monotonicity, fallback recommendation availability.

**MVP Scope**: Include

**Later Scope**: Idle Entertainment Recommendation Unit として独立、ユーザー嗜好、時間帯別テンプレート。

### UOW-005: Excuse Generation Unit

**Responsibility**: 危険圏または失敗時に、編集可能な言い訳文の下書きを生成する。LLM が使えない場合はテンプレートでフォールバックする。

**Main Components**
- Excuse Generator
- LLM Adapter Port usage

**User Stories**
- US-006: Excuse Generation
- US-009: Deadline Failure Flow, later

**Human-Degrading Effect**: 反省より先に逃げ道を確保し、失敗時の心理的負担をアプリへ外部化する。

**User Value**: 連絡文の下書きを素早く作れる。ユーザーがそのまま送るのではなく、編集可能な下書きとして扱える。

**Business Value**: テーマ性と創造性が強く、デモの記憶に残る差別化要素になる。

**Implementation Targets**
- Screens: Excuse Draft
- APIs: `POST /demo/excuse`
- Data: `ExcuseRequest`, `ExcuseDraft`, `ExcuseTone`

**PBT Candidate**: No for MVP. Template fallback can be unit-tested.

**MVP Scope**: Include

**Later Scope**: Failure Recovery Unit、トーン調整、相手別テンプレート。

### UOW-006: History and Persistence Unit

**Responsibility**: タスク、推定、結果、実績作業時間を保存し、次回の見積もり補正に使える履歴シグナルへ変換する。

**Main Components**
- Task History Repository
- Self-Deception History

**User Stories**
- US-007: History Recording
- US-002: LLM Workload Estimation, support only
- US-008: Estimation Fallback, support only

**Human-Degrading Effect**: 自分の先延ばし傾向を自分で振り返らず、履歴の解釈と補正をアプリへ任せられる。

**User Value**: 過去の成功、失敗、見積もりずれが次回の判断に反映される。

**Business Value**: 継続利用価値と将来的なパーソナライズの土台になる。AWS 上の DB 実装にも自然につながる。

**Implementation Targets**
- Screens: History Snapshot
- APIs: `POST /demo/outcome`, `GET /demo/history`
- Data: `TaskRecord`, `AnalysisRecord`, `HistoryRecord`, `HistorySignal`

**PBT Candidate**: Later. Serialization round-trip can be added in Construction if data model riskが高い場合。

**MVP Scope**: Include

**Later Scope**: ユーザー認証、長期傾向分析、履歴の可視化。

## Later Units

### LUOW-001: Idle Entertainment Recommendation Unit

MVP では Idle Planning Unit 内に含める。後続では、娯楽提案、現実逃避提案、短時間休憩提案、嗜好学習を独立 Unit として扱う。

### LUOW-002: LLM Assistance Unit

MVP では Workload Estimation Unit に LLM Adapter を含める。後続では、見積もり、言い訳、娯楽提案の共通 AI 境界として独立させる。

### LUOW-003: Failure Recovery Unit

MVP では Excuse Generation Unit の範囲に留める。後続では、期限超過後の状態整理、結果記録、次回への補正、言い訳導線を扱う。

## MVP Coverage Summary

| MVP Capability | Owning Unit |
|---|---|
| Minimal task intake | Demo Experience Unit |
| LLM-assisted workload estimation | Workload Estimation Unit |
| Latest safe start decision | Deadline Decision Unit |
| Risk band explanation | Deadline Decision Unit |
| Guilt-free idle time | Idle Planning Unit |
| Entertainment and escapism suggestions | Idle Planning Unit |
| Excuse generation | Excuse Generation Unit |
| History recording and personalization signal | History and Persistence Unit |

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | Units Generation maps PBT candidate areas to units. Enforced PBT rules apply in Functional Design, NFR Requirements, Code Generation, and Build/Test. |
