# Services

## Service Layer Strategy

MVPではフロントエンド向けにBFF的なAPIを用意し、内部ではCapability-oriented componentsを分離する。画面は少ないAPIでデモ体験を実行でき、設計上はUnit分解へ接続しやすい。

## Services

### Procrastination Demo Service

**Purpose**: MVPデモの中心サービス。タスク入力から分析結果表示までを一括で調整する。

**Responsibilities**:
- `TaskInput`を受け取る。
- 履歴シグナルを取得する。
- 作業時間を推定する。
- 最遅着手時刻、成功確率、リスク帯を算出する。
- サボってよい時間とリスク説明を作る。
- サボってよい時間に合う娯楽・休憩・現実逃避プランを作る。
- 推定結果を保存する。
- フロントエンド向けに1つの結果へまとめる。

**Orchestration Flow**:
1. Procrastination BFF API receives task input.
2. Demo Experience Orchestrator validates minimal input.
3. Task History Repository finds similar records.
4. Self-Deception History builds history signal.
5. Workload Estimator estimates workload using LLM Adapter and fallback.
6. Procrastination Deadline Engine makes the deadline decision.
7. Guilt-Free Idle Planner creates idle plan.
8. Idle Entertainment Recommender creates idle activity suggestions.
9. Guilt-Free Idle Planner integrates idle suggestions into the idle plan.
10. Risk Explanation Composer creates explanation.
11. Task History Repository stores task and analysis.
12. BFF API returns `TaskAnalysisResult` including `idleActivitySuggestions`.

### Excuse Service

**Purpose**: 失敗時または危険圏で言い訳下書きを生成する。

**Responsibilities**:
- タスク状況とリスク判定を受け取る。
- LLM Adapterへ言い訳生成を依頼する。
- LLM失敗時はテンプレートベースへフォールバックする。
- 編集可能な下書きとして返す。

**Orchestration Flow**:
1. BFF API receives excuse request.
2. Orchestrator loads task context if needed.
3. Excuse Generator calls LLM Adapter.
4. Excuse Generator falls back to template if needed.
5. BFF API returns `ExcuseDraft`.

### Outcome Recording Service

**Purpose**: 達成/失敗結果を保存し、次回推定へ接続する。

**Responsibilities**:
- タスクの実績結果を保存する。
- 実績作業時間があれば保存する。
- 次回のSelf-Deception Historyで使える形式にする。

### History Insight Service

**Purpose**: 保存された履歴を推定に使えるシグナルへ変換する。

**Responsibilities**:
- 類似タスク履歴を取得する。
- 成功/失敗傾向を要約する。
- 推定補正材料としてWorkload Estimatorへ渡す。

## BFF API Endpoints

| Endpoint | Primary Service | Internal Components |
|---|---|---|
| `POST /demo/analyze-task` | Procrastination Demo Service | History, Estimator, Engine, Idle Planner, Idle Entertainment Recommender, Risk Explanation |
| `POST /demo/excuse` | Excuse Service | Excuse Generator, LLM Adapter |
| `POST /demo/outcome` | Outcome Recording Service | Task History Repository |
| `GET /demo/history` | History Insight Service | Task History Repository, Self-Deception History |

## Screen Flow

| Screen | Responsibility | Backing API |
|---|---|---|
| Minimal Task Input | タスク名と締め切りだけを入力する | `POST /demo/analyze-task` |
| Procrastination Result | 最遅着手時刻、成功確率、リスク帯、サボってよい時間、娯楽・休憩・現実逃避プランを表示する | `POST /demo/analyze-task` |
| Excuse Draft | 言い訳文の下書きを表示・編集する | `POST /demo/excuse` |
| History Snapshot | 過去の推定・結果を表示する | `GET /demo/history` |

## Service Design Notes

- MVPではBFF APIを優先し、画面実装を単純化する。
- 内部コンポーネントはOrchestrator経由で呼び出し、直接依存を抑える。
- LLM Adapterは共有コンポーネントだが、LLM失敗時も中核体験を継続する。
- Idle Entertainment RecommenderはLLM提案を使えるが、失敗時はテンプレートへフォールバックする。
- 安全圏では娯楽/現実逃避、危険圏では短時間休憩、詰みかけでは言い訳生成への誘導を返す。
- DB保存と履歴分析を分離し、AWSデプロイ時にRepository実装を差し替えやすくする。

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | Service orchestrationを定義する文書であり、PBT実装は後続。 |
