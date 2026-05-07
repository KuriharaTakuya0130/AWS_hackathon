# Requirements Analysis

## Intent Analysis Summary

### User Request
締め切りギリギリまで着手しない人のために、「まだ始めなくてよい時間」を推定し、罪悪感を減らしながら先延ばしできるWebアプリを作る。

### Request Type
- **Type**: New Project
- **Project State**: Greenfield
- **Primary Output**: Inception成果物と、後続でMVP実装に進める要求定義

### Scope Estimate
- **Scope**: Multiple Components
- **Expected Components**:
  - タスク入力
  - LLM補助つき作業時間推定
  - 最遅着手時刻・成功確率・リスク帯算出
  - サボってよい時間の可視化
  - 言い訳生成
  - 履歴保存
  - 軽量APIと簡易DB

### Complexity Estimate
- **Complexity**: Moderate
- **Rationale**:
  - MVPとしては単一ユーザー向けの軽量Webアプリに収める。
  - ただし、普通のタスク管理ではなく「先延ばしを前提にした意思決定支援」という独自Intentがある。
  - LLMを使うが、外部AIへの強依存は避け、フォールバック可能な推定ロジックを要求する。

## Product Intent

このサービスの最重要Intentは、**先延ばしを肯定しつつ、破綻しないギリギリの開始判断を支援すること**である。

従来のタスク管理アプリは「早く始める」「効率よく終わらせる」ことを価値にする。一方、このサービスは、ユーザーが着手を先延ばしする前提を受け入れたうえで、締め切り・タスク内容・履歴から「どこまでサボれるか」を確率とリスクで示す。

このサービスが人をダメにする点は、ユーザーから「今やるべきか」を自分で判断する負担を奪い、アプリに判断を委ねさせることにある。ただし、単なる悪ふざけではなく、破綻しない範囲を明示することで、先延ばし癖を持つ人の意思決定を現実的に支援する。

## Theme Fit

### 人をダメにする効果
- ユーザーに「まだ始めなくていい」という免罪符を与える。
- サボる時間を、罪悪感ではなく可視化された許可時間として扱う。
- 判断放棄を体験の中心に置き、「アプリが決めたからまだ大丈夫」という感覚を作る。
- 間に合わなかった場合の言い訳生成により、反省よりも自己防衛を支援する。

### 実用性
- 推定は断定ではなく、成功確率・安全圏・危険圏・詰みかけとして表現する。
- ユーザーが失敗しやすい見積もりや着手遅れを可視化する。
- 履歴を蓄積し、次回以降の見積もり補助に使う。

## MVP Definition

MVPの中心体験は、**タスク入力後、最遅着手時刻とリスク帯がすぐ表示される体験**である。

### MVP Platform
- Webフロントエンド
- 軽量API
- 簡易DB

### MVP Data Persistence
- 簡易DBを使い、後続でAWSへデプロイしやすい構成にする。
- 本格的なユーザー認証はMVPスコープ外とする。

### MVP Input Policy
入力はできるだけ少なくし、初回デモで流れが速く見えることを優先する。

MVPの必須入力は以下とする。
- タスク名
- 締め切り

作業時間は以下から推定する。
- LLMによるタスク内容からの見積もり
- 過去DBの類似タスク履歴
- フォールバック用のルールベース推定またはデフォルト値

### LLM Usage Policy
LLMはMVPの機能に含める。ただし、外部AI APIへの強依存は避ける。

要求上の扱いは以下とする。
- LLMはタスク内容と過去DBから作業時間を推定する。
- LLMが利用できない場合、ルールベース推定へフォールバックする。
- LLMの出力は断定ではなく、成功確率・リスク・不確実性として表示する。
- 外部AI APIが使えない環境でも、MVPデモが破綻しないことを要求する。

## Functional Requirements

### FR-001: Task Intake
ユーザーは、タスク名と締め切りを入力できる。

### FR-002: Workload Estimation
システムは、タスク名、締め切り、過去DBの履歴をもとに作業時間を推定できる。

### FR-003: LLM-Assisted Estimation
システムは、LLMを使ってタスク内容から作業時間を推定できる。

### FR-004: Estimation Fallback
LLMが利用できない、失敗する、または信頼度が低い場合、システムはルールベース推定またはデフォルト値で作業時間を推定できる。

### FR-005: Latest Start Time Calculation
システムは、締め切り、推定作業時間、バッファ、過去傾向から最遅着手時刻を算出できる。

### FR-006: Success Probability
システムは、現在時刻、締め切り、推定作業時間、履歴から成功確率を算出できる。

### FR-007: Risk Band Display
システムは、状態を安全圏、危険圏、詰みかけなどのリスク帯として表示できる。

### FR-008: Guilt-Free Idle Time
システムは、最遅着手時刻までの「サボってよい時間」を表示できる。

### FR-009: Excuse Generation
システムは、締め切り失敗時またはデモ上の操作で、言い訳文を生成できる。

### FR-010: History Recording
システムは、タスク、推定結果、達成/失敗結果、実績作業時間を簡易DBに保存できる。

### FR-011: History-Based Personalization
システムは、過去DBの履歴を作業時間推定または成功確率の補正に利用できる。

### FR-012: Transparent Uncertainty
システムは、推定結果を断定として表示せず、成功確率、リスク帯、不確実性を併記する。

### FR-013: Demo-Oriented Flow
ユーザーは、少ない入力で短時間に主要結果を確認できる。

## Non-Functional Requirements

### NFR-001: Usability
初回ユーザーが短時間で「まだ始めなくてよいか」を理解できる画面構成にする。

### NFR-002: Demo Quality
MVPデモでは、タスク入力から最遅着手時刻、リスク帯、サボってよい時間、言い訳生成までの体験が途切れないこと。

### NFR-003: Explainability
推定結果には、主な根拠を表示する。

例:
- 締め切りまでの残り時間
- 推定作業時間
- バッファ
- 過去履歴の影響
- LLM推定またはフォールバック推定のどちらを使ったか

### NFR-004: Reliability
LLMや外部AIが利用できない場合でも、アプリの中核体験は継続できる。

### NFR-005: Maintainability
最遅着手時刻、成功確率、リスク帯判定は、UIから分離された純粋関数または独立したサービスとして設計する。

### NFR-006: Testability
推定ロジックは通常の単体テストに加え、Property-Based Testingを部分適用できる構造にする。

### NFR-007: Deployability
後続でAWSへデプロイしやすいよう、フロントエンド、軽量API、簡易DBの境界を明確にする。

### NFR-008: Safety of Expression
サービスのトーンはかなり攻める。サボり、言い訳、反省回避を前面に出す。ただし、推定は断定せず、リスク表示とフォールバック説明を含める。

## Out of Scope for Initial MVP

以下は初期MVPのスコープ外とする。

- 外部カレンダー連携
- 本格的なユーザー認証
- 外部AI APIへの強い依存
- チーム共有・共同タスク管理

「外部AI APIへの強い依存」はスコープ外だが、LLM補助そのものはMVPに含める。外部AIが使えない場合に中核体験が破綻しないことを要求する。

## User Psychology Focus

User Storiesでは、特に**判断放棄と「アプリに決めてもらう」感覚**を深掘りする。

関連して扱う心理は以下とする。
- 先延ばし
- 罪悪感
- 判断放棄
- 言い訳
- 反省回避
- 責任の外部化
- サボる時間の正当化

## Application Design Direction

Application Designでは、普通のCRUD設計ではなく、以下を独自コンポーネントとして強調する。

### Primary Component
**Procrastination Deadline Engine**

責務:
- 最遅着手時刻を算出する。
- 成功確率を算出する。
- 安全圏、危険圏、詰みかけを判定する。
- LLM推定値とフォールバック推定値を統合する。

### Supporting Components
- **Guilt-Free Idle Planner**: サボってよい時間と現実逃避プランを提案する。
- **Excuse Generator**: 失敗時の言い訳文を生成する。
- **Self-Deception History**: 過去の成功・失敗・推定履歴を蓄積する。

## Units Generation Direction

Unit分解では、審査資料のわかりやすさを最優先し、**能力単位と実装単位を併記する**。

各Unitには以下を含める。
- 責務
- 対応するユーザーストーリー
- 人をダメにする効果
- ユーザー価値
- ビジネス価値
- 実装対象となる画面
- 実装対象となるAPI
- 実装対象となるデータ
- MVPに含めるか、後続拡張に回すか

## Acceptance Criteria

### AC-001: Minimal Input Works
ユーザーがタスク名と締め切りを入力すると、推定結果画面へ進める。

### AC-002: Latest Start Time Is Shown
システムは最遅着手時刻を表示する。

### AC-003: Risk Is Not Asserted as Certainty
システムは「必ず間に合う」と断定せず、成功確率とリスク帯を表示する。

### AC-004: LLM Failure Does Not Break Demo
LLM推定が失敗しても、フォールバック推定により最遅着手時刻とリスク帯を表示できる。

### AC-005: Guilt-Free Idle Time Is Visible
システムは「サボってよい時間」を表示する。

### AC-006: Excuse Generation Exists
システムは言い訳文を生成できる。

### AC-007: History Is Stored
システムはタスク結果と推定結果を簡易DBへ保存できる。

### AC-008: Theme Fit Is Explicit
ドキュメントと画面文言は、このサービスが人をどうダメにするのかを説明できる。

## Property-Based Testing Requirements

Property-Based TestingはPartialで有効とする。以下の範囲に限定して適用する。

### Enforced PBT Rules
- **PBT-02**: シリアライズ/デシリアライズがある場合は往復性を検証する。
- **PBT-03**: 推定ロジックの不変条件を検証する。
- **PBT-07**: タスク入力や締め切りなど、ドメインに沿ったジェネレータを使う。
- **PBT-08**: PBTの失敗時に再現できるようseedを扱う。
- **PBT-09**: 実装言語に応じたPBTフレームワークを選定する。

### Candidate Properties
- 成功確率は常に0から100の範囲に収まる。
- 締め切りまでの残り時間が短くなるほど、同一条件で成功確率は上がらない。
- 推定作業時間が長くなるほど、同一条件で成功確率は上がらない。
- バッファが大きくなるほど、同一条件で危険度は上がらない。
- 最遅着手時刻は締め切りより後にならない。

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Disabled | PoC/プロトタイプとして、Requirements Analysisではブロッキング制約にしない。 |
| Property-Based Testing | Enabled Partial | 推定ロジックとシリアライズ等の限定範囲で適用する。Requirements Analysis時点では対象プロパティ候補を明記済み。 |

## Requirements Approval Scope

このRequirements Analysisを承認すると、次のUser Storiesでは以下を中心に具体化する。

- 先延ばしを前提にしたユーザー心理
- 判断放棄とアプリへの責任委譲
- 言い訳生成と反省回避
- MVPデモで伝わる体験
- Unit分解に接続できるストーリー構造
