# Component Methods

## Method Signature Style

この文書では実装言語に依存しない擬似TypeScript形式で主要メソッドを定義する。詳細なアルゴリズムとビジネスルールはConstructionのFunctional Designで定義する。

## Shared Types

```typescript
type TaskInput = {
  title: string;
  deadline: string;
};

type WorkloadEstimate = {
  estimatedMinutes: number;
  confidence: number;
  source: "llm" | "fallback" | "history-adjusted";
  rationale: string[];
};

type DeadlineDecision = {
  latestStartAt: string;
  successProbability: number;
  riskBand: "safe" | "risky" | "nearly-doomed";
};

type IdlePlan = {
  idleMinutes: number;
  displayMessage: string;
  activitySuggestions: IdleActivitySuggestion[];
};

type IdleActivitySuggestion = {
  planName: string;
  category: "break" | "entertainment" | "escapism" | "excuse-prep";
  recommendedMinutes: number;
  returnAt: string;
  source: "llm" | "template";
};

type RiskExplanation = {
  headline: string;
  reasons: string[];
  uncertaintyNote: string;
};

type ExcuseDraft = {
  text: string;
  tone: "humorous" | "careful" | "desperate";
  disclaimer: string;
};
```

## Demo Experience Orchestrator

| Method | Input | Output | Purpose |
|---|---|---|---|
| `analyzeTask(input)` | `TaskInput` | `TaskAnalysisResult` | MVPの中心体験を一括実行する |
| `generateExcuse(taskId, context)` | `string, ExcuseContext` | `ExcuseDraft` | 言い訳生成を調整する |
| `recordOutcome(taskId, outcome)` | `string, OutcomeInput` | `HistoryRecord` | 達成/失敗結果を保存する |

### Notes
- Orchestratorが各コンポーネントを呼び出す。
- コンポーネント同士の直接依存を抑える。

## Procrastination Deadline Engine

| Method | Input | Output | Purpose |
|---|---|---|---|
| `calculateLatestStart(deadline, estimate, bufferPolicy)` | `string, WorkloadEstimate, BufferPolicy` | `string` | 最遅着手時刻を算出する |
| `calculateSuccessProbability(deadline, estimate, now, historySignal)` | `string, WorkloadEstimate, string, HistorySignal` | `number` | 成功確率を0から100で算出する |
| `classifyRiskBand(probability, latestStartAt, now)` | `number, string, string` | `RiskBand` | 安全圏/危険圏/詰みかけを判定する |
| `makeDecision(input)` | `DeadlineDecisionInput` | `DeadlineDecision` | 中核判定をまとめて返す |

### PBT Candidates
- 成功確率は0から100の範囲に収まる。
- 最遅着手時刻は締め切りより後にならない。
- 推定作業時間が長くなるほど、同一条件で成功確率は上がらない。

## LLM Adapter

| Method | Input | Output | Purpose |
|---|---|---|---|
| `estimateWorkload(prompt)` | `WorkloadPrompt` | `LLMResult<WorkloadEstimate>` | LLMに作業時間推定を依頼する |
| `generateExcuse(prompt)` | `ExcusePrompt` | `LLMResult<ExcuseDraft>` | LLMに言い訳文生成を依頼する |
| `isAvailable()` | none | `boolean` | LLM利用可否を返す |

### Notes
- LLM失敗は例外で隠さず、呼び出し元がフォールバック可能な結果として返す。

## Workload Estimator

| Method | Input | Output | Purpose |
|---|---|---|---|
| `estimate(input, historySignal)` | `TaskInput, HistorySignal` | `WorkloadEstimate` | LLM推定と履歴補正を統合する |
| `fallbackEstimate(input, historySignal)` | `TaskInput, HistorySignal` | `WorkloadEstimate` | LLM失敗時の推定を返す |
| `mergeEstimate(llmEstimate, fallbackEstimate, historySignal)` | `optional WorkloadEstimate, WorkloadEstimate, HistorySignal` | `WorkloadEstimate` | 推定結果を統合する |

### PBT Candidates
- フォールバック推定は正の作業時間を返す。
- 信頼度は0から100の範囲に収まる。

## Guilt-Free Idle Planner

| Method | Input | Output | Purpose |
|---|---|---|---|
| `calculateIdleTime(now, latestStartAt)` | `string, string` | `number` | サボってよい分数を算出する |
| `composeIdlePlan(decision, suggestions)` | `DeadlineDecision, IdleActivitySuggestion[]` | `IdlePlan` | サボってよい時間と娯楽提案を統合する |

### PBT Candidates
- 最遅着手時刻が近づくほど、サボってよい時間は増えない。
- サボってよい時間は負数として表示しない。

## Idle Entertainment Recommender

| Method | Input | Output | Purpose |
|---|---|---|---|
| `recommend(context)` | `IdleRecommendationContext` | `IdleActivitySuggestion[]` | 残り時間、リスク帯、タスク内容に応じた娯楽・休憩・現実逃避プランを推薦する |
| `fallbackRecommend(context)` | `IdleRecommendationContext` | `IdleActivitySuggestion[]` | LLM失敗時にテンプレートベースの提案を返す |
| `classifySuggestionMode(riskBand)` | `RiskBand` | `"free-escape" | "short-break" | "excuse-prep"` | リスク帯に応じて提案モードを決める |

### Notes
- 安全圏では娯楽/現実逃避を積極提案する。
- 危険圏では短時間休憩に制限する。
- 詰みかけでは言い訳生成へ誘導する。
- 出力にはプラン名、推奨時間、戻るべき時刻を含める。

## Risk Explanation Composer

| Method | Input | Output | Purpose |
|---|---|---|---|
| `compose(decision, estimate, historySignal)` | `DeadlineDecision, WorkloadEstimate, HistorySignal` | `RiskExplanation` | 断定しない説明を生成する |
| `composeUncertaintyNote(estimate)` | `WorkloadEstimate` | `string` | LLM/フォールバック由来の不確実性を説明する |

## Excuse Generator

| Method | Input | Output | Purpose |
|---|---|---|---|
| `generate(task, decision, tone)` | `TaskContext, DeadlineDecision, ExcuseTone` | `ExcuseDraft` | 言い訳下書きを生成する |
| `fallbackExcuse(task, decision, tone)` | `TaskContext, DeadlineDecision, ExcuseTone` | `ExcuseDraft` | LLM失敗時のテンプレート言い訳を返す |

## Self-Deception History

| Method | Input | Output | Purpose |
|---|---|---|---|
| `buildHistorySignal(taskInput)` | `TaskInput` | `HistorySignal` | 類似履歴を推定材料へ変換する |
| `summarizePattern(records)` | `HistoryRecord[]` | `HistorySignal` | 成功/失敗傾向を要約する |

## Task History Repository

| Method | Input | Output | Purpose |
|---|---|---|---|
| `saveTask(task)` | `TaskRecord` | `TaskRecord` | タスクを保存する |
| `saveAnalysis(taskId, analysis)` | `string, TaskAnalysisResult` | `AnalysisRecord` | 推定結果を保存する |
| `saveOutcome(taskId, outcome)` | `string, OutcomeInput` | `HistoryRecord` | 達成/失敗結果を保存する |
| `findSimilar(taskInput)` | `TaskInput` | `HistoryRecord[]` | 類似履歴を返す |

## Procrastination BFF API

| Method | Input | Output | Purpose |
|---|---|---|---|
| `POST /demo/analyze-task` | `TaskInput` | `TaskAnalysisResult` | デモ体験の主API |
| `POST /demo/excuse` | `ExcuseRequest` | `ExcuseDraft` | 言い訳生成API |
| `POST /demo/outcome` | `OutcomeInput` | `HistoryRecord` | 結果保存API |
| `GET /demo/history` | query | `HistoryRecord[]` | 履歴表示API |

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | PBT対象候補は明記済み。PBTルールの強制検証は後続設計・コードで実施する。 |
