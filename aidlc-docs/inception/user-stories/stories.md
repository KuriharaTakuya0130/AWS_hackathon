# User Stories

## Story Strategy

Story分解は **Hybrid: User Journey-Based + Capability-Based** とする。

ストーリーは、MVPデモで自然に見える順序に並べる。同時に、Units Generationで使えるよう、各ストーリーにCapabilityとUnit候補を併記する。

優先度は以下で分類する。

- **Demo Critical**: デモで必ず見せるべき中核体験
- **MVP Core**: MVPとして実装価値が高い中核機能
- **Later**: 後続拡張

## Capability Definitions

| Capability | Description | 人をダメにする効果 |
|---|---|---|
| Minimal Task Intake | 最小入力でタスクを登録する | 面倒な入力すら避けられる |
| LLM Workload Estimation | タスク内容から作業時間を推定する | 自分で見積もらずに済む |
| Latest Start Decision | 最遅着手時刻と成功確率を示す | 着手判断をアプリへ委ねられる |
| Guilt-Free Idle Planning | サボってよい時間を可視化する | サボりを罪悪感なく正当化できる |
| Risk Band Explanation | 安全圏/危険圏/詰みかけを説明する | 危険になるまで反省を先送りできる |
| Excuse Generation | 失敗時の言い訳を生成する | 反省より先に逃げ道を確保できる |
| Self-Deception History | 過去の推定・結果を保存する | 自分の傾向すらアプリに任せられる |
| Resilient Estimation | LLM失敗時も推定を継続する | 判断の外部委託が途切れない |

## Stories

### US-001: Minimal Task Intake

- **Priority**: Demo Critical
- **Persona**: サボり時間を正当化したい人, 見積もりが甘く毎回焦る人
- **Capability**: Minimal Task Intake
- **Unit Candidate**: Task Intake Unit

#### Story
サボる時間を正当化したいユーザーとして、タスク名と締め切りだけを入力したい。なぜなら、細かい情報入力で現実に向き合う前に、まず「まだサボれるか」を知りたいから。

#### Acceptance Criteria
- ユーザーはタスク名を入力できる。
- ユーザーは締め切り日時を入力できる。
- 見積もり作業時間を手入力しなくても次へ進める。
- 入力不足がある場合、タスク名または締め切りの不足だけを明示する。

#### Given/When/Then
- **Given** ユーザーがタスク名と締め切りを入力している
- **When** 推定を開始する
- **Then** システムは作業時間推定と最遅着手時刻算出へ進む

#### Theme Fit
面倒な入力を減らすことで、ユーザーが現実と向き合う時間すら短縮する。

#### Business Value
デモの最初の体験が速くなり、MVP完成度が伝わりやすい。

#### INVEST Check
- Independent: Yes
- Negotiable: Yes
- Valuable: Yes
- Estimable: Yes
- Small: Yes
- Testable: Yes

### US-002: LLM Workload Estimation

- **Priority**: Demo Critical
- **Persona**: サボり時間を正当化したい人, 見積もりが甘く毎回焦る人
- **Capability**: LLM Workload Estimation
- **Unit Candidate**: Workload Estimation Unit

#### Story
見積もりが苦手なユーザーとして、タスク内容から作業時間を推定してほしい。なぜなら、自分で作業量を考えると面倒になり、結局判断を先送りしてしまうから。

#### Acceptance Criteria
- システムはタスク名から作業時間を推定する。
- システムは過去DBの類似履歴がある場合、推定に反映する。
- 推定結果には、LLM推定かフォールバック推定かを表示する。
- 推定結果は断定せず、不確実性を含めて表示する。

#### Given/When/Then
- **Given** ユーザーがタスク名と締め切りを入力している
- **When** システムが作業時間を推定する
- **Then** 推定作業時間と推定根拠が表示される

#### Theme Fit
ユーザーは自分で見積もる責任を放棄し、アプリに作業量判断を委ねられる。

#### Business Value
このサービスが普通のタスク管理ではなく、先延ばし前提の判断支援であることを示す。

#### INVEST Check
- Independent: Yes
- Negotiable: Yes
- Valuable: Yes
- Estimable: Yes
- Small: Yes
- Testable: Yes

### US-003: Latest Start Decision

- **Priority**: Demo Critical
- **Persona**: サボり時間を正当化したい人, 見積もりが甘く毎回焦る人
- **Capability**: Latest Start Decision
- **Unit Candidate**: Procrastination Deadline Engine Unit

#### Story
サボる時間を正当化したいユーザーとして、最遅着手時刻を知りたい。なぜなら、まだ始めなくてよい根拠があれば、罪悪感を減らしてサボれるから。

#### Acceptance Criteria
- システムは最遅着手時刻を表示する。
- システムは成功確率を0から100の範囲で表示する。
- システムは「必ず間に合う」と断定しない。
- 最遅着手時刻は締め切りより後にならない。

#### Given/When/Then
- **Given** 推定作業時間と締め切りが存在する
- **When** システムが最遅着手時刻を算出する
- **Then** ユーザーはいつまで始めなくてよいかを確認できる

#### Theme Fit
着手判断をユーザーから奪い、「アプリがまだ大丈夫と言ったからサボる」という判断放棄を作る。

#### Business Value
サービスのIntentとデモの中核であり、Unit分解の中心となる。

#### INVEST Check
- Independent: Yes
- Negotiable: Yes
- Valuable: Yes
- Estimable: Yes
- Small: Yes
- Testable: Yes

### US-004: Guilt-Free Idle Time

- **Priority**: Demo Critical
- **Persona**: サボり時間を正当化したい人
- **Capability**: Guilt-Free Idle Planning
- **Unit Candidate**: Idle Planning Unit

#### Story
サボる時間を楽しみたいユーザーとして、着手までの「サボってよい時間」を見たい。なぜなら、罪悪感なしに娯楽や現実逃避を楽しみたいから。

#### Acceptance Criteria
- システムは現在時刻から最遅着手時刻までの時間を表示する。
- システムはサボってよい時間をポジティブな表現で提示する。
- サボってよい時間がない場合、危険圏または詰みかけとして表示する。
- 表示はタスク完了を保証しない。

#### Theme Fit
サボりを「悪いこと」ではなく、アプリが認めた時間として扱う。

#### Business Value
テーマ適合性が強く、デモで一目で独自性が伝わる。

#### INVEST Check
- Independent: Yes
- Negotiable: Yes
- Valuable: Yes
- Estimable: Yes
- Small: Yes
- Testable: Yes

### US-005: Risk Band Explanation

- **Priority**: MVP Core
- **Persona**: サボり時間を正当化したい人, 見積もりが甘く毎回焦る人, 言い訳で心理的に逃げたい人
- **Capability**: Risk Band Explanation
- **Unit Candidate**: Risk Explanation Unit

#### Story
締め切りリスクを知りたいユーザーとして、安全圏、危険圏、詰みかけを見たい。なぜなら、まだサボれるのか、そろそろ現実へ戻るべきかを自分で考えたくないから。

#### Acceptance Criteria
- システムは安全圏、危険圏、詰みかけのいずれかを表示する。
- システムは成功確率とリスク帯を併記する。
- システムはリスク帯の根拠を簡潔に表示する。
- 危険圏でも、間に合う可能性と失敗可能性を両方示す。

#### Theme Fit
ユーザーは危険圏まで反省を先送りし、リスク判断すらアプリに任せられる。

#### Business Value
断定しないAI/ロジック推定として、実用性と安全性を補強する。

#### INVEST Check
- Independent: Yes
- Negotiable: Yes
- Valuable: Yes
- Estimable: Yes
- Small: Yes
- Testable: Yes

### US-006: Excuse Generation

- **Priority**: Demo Critical
- **Persona**: サボり時間を正当化したい人, 言い訳で心理的に逃げたい人
- **Capability**: Excuse Generation
- **Unit Candidate**: Excuse Generation Unit

#### Story
締め切りに間に合わなかったユーザーとして、言い訳文を生成したい。なぜなら、反省する前にまず心理的な逃げ道を確保したいから。

#### Acceptance Criteria
- システムは言い訳文を生成できる。
- 言い訳文はユーザーがそのまま確認・編集できる下書きとして扱う。
- 言い訳文はタスク名や締め切り状況を反映する。
- 言い訳生成はユーモアとデモ性を持つが、結果の責任をアプリが保証しない。

#### Given/When/Then
- **Given** ユーザーが失敗時または危険圏で言い訳生成を選ぶ
- **When** システムが言い訳文を生成する
- **Then** ユーザーは編集可能な言い訳の下書きを確認できる

#### Theme Fit
反省より先に言い訳を出すことで、責任から逃げる体験を提供する。

#### Business Value
ハッカソンテーマへの創造性とデモ映えを強化する。

#### INVEST Check
- Independent: Yes
- Negotiable: Yes
- Valuable: Yes
- Estimable: Yes
- Small: Yes
- Testable: Yes

### US-007: History Recording

- **Priority**: MVP Core
- **Persona**: 見積もりが甘く毎回焦る人
- **Capability**: Self-Deception History
- **Unit Candidate**: History Unit

#### Story
見積もりが甘いユーザーとして、過去の推定と結果を保存したい。なぜなら、自分の先延ばし傾向を次回の推定に反映してほしいから。

#### Acceptance Criteria
- システムはタスク名、締め切り、推定作業時間、推定結果を保存する。
- システムは達成または失敗の結果を保存できる。
- システムは後続推定で履歴を参照できる。
- 本格的なユーザー認証は不要とする。

#### Theme Fit
ユーザーは自分の傾向分析すらアプリに任せ、自己理解の努力を省略できる。

#### Business Value
継続利用価値とAWSデプロイ拡張の土台になる。

#### INVEST Check
- Independent: Yes
- Negotiable: Yes
- Valuable: Yes
- Estimable: Yes
- Small: Yes
- Testable: Yes

### US-008: Estimation Fallback

- **Priority**: MVP Core
- **Persona**: 見積もりが甘く毎回焦る人
- **Capability**: Resilient Estimation
- **Unit Candidate**: Workload Estimation Unit

#### Story
LLM推定を使うユーザーとして、LLMが失敗しても推定結果を見たい。なぜなら、外部AIの都合で「まだサボれるか」の判断が止まると困るから。

#### Acceptance Criteria
- LLMが失敗した場合、システムはルールベースまたはデフォルト値で推定する。
- フォールバックが使われた場合、ユーザーにその旨を表示する。
- フォールバック時も最遅着手時刻とリスク帯を表示できる。
- フォールバック推定も断定ではなく不確実性を表示する。

#### Theme Fit
判断の外部委託が途切れず、ユーザーは自分で考える状態へ戻されない。

#### Business Value
デモ安定性と実装信頼性を高め、外部AI強依存を避ける。

#### INVEST Check
- Independent: Yes
- Negotiable: Yes
- Valuable: Yes
- Estimable: Yes
- Small: Yes
- Testable: Yes

### US-009: Deadline Failure Flow

- **Priority**: Later
- **Persona**: 言い訳で心理的に逃げたい人
- **Capability**: Excuse Generation
- **Unit Candidate**: Failure Recovery Unit

#### Story
締め切りを過ぎたユーザーとして、失敗後の状態を整理したい。なぜなら、反省より先に次にどう逃げるかを知りたいから。

#### Acceptance Criteria
- システムは締め切り超過状態を表示できる。
- システムは言い訳生成への導線を表示できる。
- システムは失敗結果を履歴に保存できる。
- システムは後続拡張として再見積もりや再スケジュールの余地を残す。

#### Theme Fit
失敗後の反省を遅らせ、まず言い訳と後始末へユーザーを誘導する。

#### Business Value
MVP後に継続体験を広げる拡張候補になる。

#### INVEST Check
- Independent: Yes
- Negotiable: Yes
- Valuable: Yes
- Estimable: Yes
- Small: Yes
- Testable: Yes

## Story-to-Capability and Unit Candidate Map

| Story | Priority | Capability | Unit Candidate | MVP Scope |
|---|---|---|---|---|
| US-001 Minimal Task Intake | Demo Critical | Minimal Task Intake | Task Intake Unit | Include |
| US-002 LLM Workload Estimation | Demo Critical | LLM Workload Estimation | Workload Estimation Unit | Include |
| US-003 Latest Start Decision | Demo Critical | Latest Start Decision | Procrastination Deadline Engine Unit | Include |
| US-004 Guilt-Free Idle Time | Demo Critical | Guilt-Free Idle Planning | Idle Planning Unit | Include |
| US-005 Risk Band Explanation | MVP Core | Risk Band Explanation | Risk Explanation Unit | Include |
| US-006 Excuse Generation | Demo Critical | Excuse Generation | Excuse Generation Unit | Include |
| US-007 History Recording | MVP Core | Self-Deception History | History Unit | Include |
| US-008 Estimation Fallback | MVP Core | Resilient Estimation | Workload Estimation Unit | Include |
| US-009 Deadline Failure Flow | Later | Excuse Generation | Failure Recovery Unit | Later |

## Persona-to-Story Map

| Persona | Stories |
|---|---|
| サボり時間を正当化したい人 | US-001, US-002, US-003, US-004, US-005, US-006 |
| 見積もりが甘く毎回焦る人 | US-001, US-002, US-003, US-005, US-007, US-008 |
| 言い訳で心理的に逃げたい人 | US-005, US-006, US-009 |

## INVEST Summary

| Criterion | Result | Notes |
|---|---|---|
| Independent | Pass | 各ストーリーは個別のユーザー価値を持つ。 |
| Negotiable | Pass | 実装詳細ではなく、ユーザー価値と体験を中心に記述している。 |
| Valuable | Pass | 各ストーリーにユーザー価値、テーマ価値、ビジネス価値がある。 |
| Estimable | Pass | Unit候補と受け入れ基準により見積もり可能。 |
| Small | Pass | MVPに含めるストーリーは画面/API単位に分解可能。 |
| Testable | Pass | すべてのストーリーにAcceptance Criteriaを付与済み。 |

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | User Stories artifact does not produce design, code, or test implementation subject to PBT enforcement. PBT candidates remain documented in requirements for later design/code stages. |
