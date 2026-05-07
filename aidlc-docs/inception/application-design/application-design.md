# Application Design

## Summary

このApplication Designは、締め切りギリギリまで着手しない人に「まだサボってよいか」を示すMVP Webアプリの高レベル設計である。

設計方針は **Capability-first + MVP service boundaries** とする。つまり、審査向けには「人をダメにする能力」を中心に説明し、実装向けにはBFF API、Orchestrator、Repository、LLM Adapterなどの境界を明確にする。

## Design Goals

- 普通のタスク管理アプリに見えない独自コンポーネントを定義する。
- 各コンポーネントに「人をダメにする効果」と「ビジネス価値」を持たせる。
- LLMを使うが、外部AIへの強依存を避ける。
- Orchestrator中心で依存関係を制御する。
- Units Generationに接続できるCapability/Component対応を作る。
- PBT対象となる純粋関数候補を明確にする。

## Core Components

| Component | Role |
|---|---|
| Demo Experience Orchestrator | MVP体験全体を調整する |
| Procrastination Deadline Engine | 最遅着手時刻、成功確率、リスク帯を算出する |
| LLM Adapter | LLM推定と言い訳生成を共通化する |
| Workload Estimator | LLM推定とフォールバック推定を統合する |
| Guilt-Free Idle Planner | サボってよい時間を計算する |
| Idle Entertainment Recommender | サボってよい時間に合った娯楽・休憩・現実逃避プランを推薦する |
| Risk Explanation Composer | リスクと推定根拠を説明する |
| Excuse Generator | 言い訳下書きを生成する |
| Self-Deception History | 履歴をドメイン信号へ変換する |
| Task History Repository | 簡易DBへの保存・検索を担当する |
| Procrastination BFF API | フロントエンド向けAPIを提供する |

## MVP Demo Flow

```text
1. User enters task title and deadline.
2. BFF API sends request to Demo Experience Orchestrator.
3. Orchestrator loads similar history.
4. Workload Estimator estimates work time using LLM Adapter.
5. If LLM fails, Workload Estimator uses fallback.
6. Deadline Engine calculates latest start time, success probability, and risk band.
7. Idle Planner calculates guilt-free idle time.
8. Idle Entertainment Recommender recommends entertainment, break, or escapism plans.
9. Idle Planner integrates recommendations into the idle plan.
10. Risk Explanation Composer explains uncertainty and risk.
11. Repository stores analysis result.
12. User sees result screen.
13. User can generate excuse draft if desired.
```

## Screen Responsibilities

| Screen | Responsibilities |
|---|---|
| Minimal Task Input | タスク名と締め切りだけを受け取る |
| Procrastination Result | 最遅着手時刻、成功確率、リスク帯、サボってよい時間、娯楽・休憩・現実逃避プランを表示する |
| Excuse Draft | 言い訳下書きを生成・表示する |
| History Snapshot | 過去の推定、結果、傾向を表示する |

## API Boundary

MVPではBFF的APIを用意する。

| Endpoint | Purpose |
|---|---|
| `POST /demo/analyze-task` | タスク入力から分析結果までを返す |
| `POST /demo/excuse` | 言い訳下書きを返す |
| `POST /demo/outcome` | 達成/失敗結果を保存する |
| `GET /demo/history` | 履歴スナップショットを返す |

## PBT Design Candidates

| Candidate | Reason |
|---|---|
| Latest start calculation | 締め切りより後にならないことを検証できる |
| Success probability calculation | 0から100の範囲を保証できる |
| Risk band classification | 境界値を検証できる |
| Idle time calculation | 時間経過に対する単調性を検証できる |
| Idle entertainment fallback recommendation | LLMなしでも提案を返せることを検証できる |
| Fallback estimate calculation | LLMなしでも正の推定値を返すことを検証できる |

## Human-Degrading Effects by Design

| Design Element | Effect |
|---|---|
| Minimal input | 入力の面倒さからも逃げられる |
| LLM Workload Estimator | 自分で見積もらなくてよい |
| Deadline Engine | 着手判断をアプリへ委ねられる |
| Idle Planner | サボる罪悪感が減る |
| Idle Entertainment Recommender | 現実逃避の選択までアプリに委ねられる |
| Risk Explanation | 危険判断を自分でしなくてよい |
| Excuse Generator | 反省より先に言い訳できる |
| History | 自分の傾向分析すら任せられる |

## Business Value by Design

| Design Element | Value |
|---|---|
| BFF API | MVPデモを短期間で作りやすい |
| Orchestrator | 体験を安定して統合できる |
| Pure Engine | テストしやすく信頼性を示せる |
| LLM Adapter | 外部AI利用を差し替えやすい |
| Idle Entertainment Recommender | サボってよい時間をデモで伝わる体験に変える |
| Repository split | AWSデプロイ時にDB実装を差し替えやすい |
| Capability mapping | Units Generationと審査説明に接続しやすい |

## Artifact Index

- `components.md`: コンポーネント定義と責務
- `component-methods.md`: 主要メソッドと入出力
- `services.md`: サービス層とBFF APIのオーケストレーション
- `component-dependency.md`: 依存関係、データフロー、Unit候補

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | PBT候補を設計で明示した。PBT強制ルールはFunctional Design以降で検証する。 |
