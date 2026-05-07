# Components

## Design Style

Application Designは **Hybrid: Capability-first + MVP service boundaries** で構成する。

主構造は「人をダメにする能力」単位とし、MVP実装に必要なAPI、Repository、BFF、Adapterを併記する。これにより、審査資料では独自性を示し、後続のConstructionでは実装単位へ落とし込める。

## Component Overview

| Component | Purpose | Capability | Human-Degrading Effect | Business Value |
|---|---|---|---|---|
| Demo Experience Orchestrator | MVPデモの一連の体験を調整する | Demo Flow | ユーザーが判断せず、流れに乗るだけでサボる許可を得る | デモ完成度と実装統合を高める |
| Procrastination Deadline Engine | 最遅着手時刻、成功確率、リスク帯を算出する | Latest Start Decision | 着手判断をアプリへ委ねさせる | サービスの中核価値を担う |
| LLM Adapter | LLMを使った推定・生成を共通化する | LLM Assistance | 見積もりや言い訳を自分で考えなくてよくする | 外部AI利用を抽象化し拡張しやすくする |
| Workload Estimator | 作業時間を推定し、LLM結果とフォールバック結果を統合する | LLM Workload Estimation | 自分で作業量を見積もる責任を放棄できる | 推定精度とデモ安定性を両立する |
| Guilt-Free Idle Planner | サボってよい時間を計算・提示する | Guilt-Free Idle Planning | サボりを罪悪感なく正当化する | テーマ適合性とデモ訴求力を高める |
| Idle Entertainment Recommender | サボってよい時間に合った娯楽・休憩・現実逃避プランを推薦する | Idle Entertainment Recommendation | 現実逃避の選択までアプリに委ねさせる | デモ体験の独自性とテーマ適合性を高める |
| Risk Explanation Composer | リスク帯と推定根拠をユーザー向けに説明する | Risk Band Explanation | 危険判断も自分でせずに済む | 断定しない推定として信頼性を補強する |
| Excuse Generator | 失敗時や危険時の言い訳文を生成する | Excuse Generation | 反省より先に逃げ道を確保する | 創造性とデモ映えを強化する |
| Self-Deception History | 履歴から傾向・類似性を解釈する | Self-Deception History | 自己分析すらアプリへ任せられる | 継続利用価値を作る |
| Task History Repository | タスク、推定、結果、履歴を保存・検索する | Persistence | 自分の先延ばし履歴をアプリに蓄積させる | 簡易DBとAWS拡張の土台になる |
| Procrastination BFF API | フロントエンド向けにデモ体験APIを提供する | API Boundary | 画面は複雑な判断を意識せず結果だけ受け取る | MVP実装を単純化しUXを安定させる |

## Component Details

### Demo Experience Orchestrator

**Purpose**: タスク入力から推定、リスク表示、サボってよい時間、言い訳生成、履歴保存までのMVP体験を調整する。

**Responsibilities**:
- BFF APIからの要求を受け取り、内部コンポーネントを順に呼び出す。
- コンポーネント間の直接依存を減らす。
- LLM失敗時もフォールバック推定でデモ体験を継続する。
- 画面が必要な結果をまとめて返す。

**Interfaces**:
- Analyze task request
- Generate excuse request
- Record outcome request

**Unit Connection**:
- Demo Flow Unit
- Task Intake Unit

### Procrastination Deadline Engine

**Purpose**: 最遅着手時刻、成功確率、リスク帯の算出に責務を限定する中核エンジン。

**Responsibilities**:
- 推定作業時間と締め切りから最遅着手時刻を算出する。
- 成功確率を0から100の範囲で算出する。
- 安全圏、危険圏、詰みかけを判定する。
- PBT対象となる純粋関数候補を提供する。

**Not Responsible For**:
- LLM呼び出し
- ユーザー向け説明文の生成
- サボりプラン生成
- DB保存

**Unit Connection**:
- Procrastination Deadline Engine Unit

### LLM Adapter

**Purpose**: Workload EstimationとExcuse Generationで使うLLMアクセスを共通化する。

**Responsibilities**:
- タスク内容から作業時間推定を依頼する。
- 言い訳文生成を依頼する。
- LLM利用不可や失敗を明示的な結果として返す。
- 外部AI APIへの強依存を避けるため、呼び出し境界を抽象化する。

**Unit Connection**:
- LLM Integration Unit
- Workload Estimation Unit
- Excuse Generation Unit

### Workload Estimator

**Purpose**: タスク名、締め切り、履歴から作業時間を推定し、LLM結果とフォールバック結果を統合する。

**Responsibilities**:
- LLM Adapterへ推定を依頼する。
- Task History Repositoryから類似履歴を受け取る。
- LLM失敗時にルールベースまたはデフォルト推定へフォールバックする。
- 推定根拠と信頼度を返す。

**Unit Connection**:
- Workload Estimation Unit

### Guilt-Free Idle Planner

**Purpose**: 最遅着手時刻までの「サボってよい時間」を計算し、Idle Entertainment Recommenderの推薦結果をIdle Planへ統合する。

**Responsibilities**:
- 現在時刻と最遅着手時刻からサボってよい時間を計算する。
- サボってよい時間がない場合は危険状態として扱う。
- Idle Entertainment Recommenderから娯楽・休憩・現実逃避プランを受け取る。
- サボってよい時間、戻るべき時刻、推薦プランをまとめて表示材料にする。
- PBT対象となる純粋関数候補を提供する。

**Unit Connection**:
- Idle Planning Unit

### Idle Entertainment Recommender

**Purpose**: サボってよい時間、リスク帯、タスク内容に応じて、娯楽・休憩・現実逃避プランを推薦する。

**Responsibilities**:
- LLMで娯楽・休憩・現実逃避プランを生成する。
- LLM利用不可時は時間枠とリスク帯に応じたテンプレートへフォールバックする。
- 安全圏では娯楽/現実逃避を積極提案する。
- 危険圏では短時間休憩に制限する。
- 詰みかけでは言い訳生成へ誘導する。
- プラン名、推奨時間、戻るべき時刻を出力する。

**Human-Degrading Effect**:
- ユーザーは「何をして現実逃避するか」まで自分で選ばなくてよくなる。

**Business Value**:
- 「サボってよい時間」を単なる数値ではなく、デモで伝わる体験に変える。

**Unit Connection**:
- MVPではIdle Planning Unitに含める。
- 後続拡張でIdle Entertainment Recommendation Unitとして独立できる。

### Risk Explanation Composer

**Purpose**: Engineの数値結果を、断定しないユーザー向け説明へ変換する。

**Responsibilities**:
- 成功確率、リスク帯、推定根拠を説明文へ変換する。
- 「必ず間に合う」と断定しない表現を維持する。
- LLM推定かフォールバック推定かを表示材料に含める。

**Unit Connection**:
- Risk Explanation Unit

### Excuse Generator

**Purpose**: 締め切り失敗時や危険圏で、編集可能な言い訳下書きを生成する。

**Responsibilities**:
- LLM Adapterを使って言い訳文を生成する。
- LLM利用不可時はテンプレートベースの言い訳を返す。
- 結果は下書きとして扱い、責任を保証しない。

**Unit Connection**:
- Excuse Generation Unit

### Self-Deception History

**Purpose**: 履歴をドメイン観点で解釈し、推定や表示に使える傾向情報を作る。

**Responsibilities**:
- 類似タスク候補をRepositoryから受け取る。
- 過去の成功/失敗、推定作業時間、実績作業時間の傾向を要約する。
- Workload Estimatorへ履歴補正材料を提供する。

**Unit Connection**:
- History Insight Unit

### Task History Repository

**Purpose**: タスク、推定結果、実績結果、言い訳生成履歴を簡易DBへ保存・検索する。

**Responsibilities**:
- タスク履歴を保存する。
- 推定結果を保存する。
- 達成/失敗結果を保存する。
- 類似履歴検索に必要なデータを返す。

**Unit Connection**:
- Persistence Unit

### Procrastination BFF API

**Purpose**: フロントエンド向けに、デモ体験に必要なデータをまとめて返す。

**Responsibilities**:
- `analyzeTask` 相当のデモ体験APIを提供する。
- `generateExcuse` 相当の言い訳生成APIを提供する。
- `recordOutcome` 相当の履歴保存APIを提供する。
- 内部コンポーネントの複雑さを画面から隠す。

**Unit Connection**:
- BFF API Unit

## PBT Pure Function Candidates

| Function Area | Candidate |
|---|---|
| Deadline Engine | latest start time calculation |
| Deadline Engine | success probability range and monotonicity |
| Deadline Engine | risk band classification |
| Idle Planner | allowed idle time calculation |
| Idle Entertainment Recommender | recommendation selection fallback |
| Workload Estimator | fallback estimate calculation |

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | PBT候補を特定したが、強制ルールの検証はFunctional Design以降で実施する。 |
