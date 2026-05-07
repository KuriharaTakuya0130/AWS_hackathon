# Unit of Work Story Map

## Story Coverage Matrix

| Story | Priority | Owning Unit | Supporting Units | MVP Scope |
|---|---|---|---|---|
| US-001 Minimal Task Intake | Demo Critical | Demo Experience Unit | History and Persistence Unit | Include |
| US-002 LLM Workload Estimation | Demo Critical | Workload Estimation Unit | Demo Experience Unit, History and Persistence Unit | Include |
| US-003 Latest Start Decision | Demo Critical | Deadline Decision Unit | Demo Experience Unit, Workload Estimation Unit | Include |
| US-004 Guilt-Free Idle Time | Demo Critical | Idle Planning Unit | Demo Experience Unit, Deadline Decision Unit | Include |
| US-005 Risk Band Explanation | MVP Core | Deadline Decision Unit | Demo Experience Unit, Idle Planning Unit | Include |
| US-006 Excuse Generation | Demo Critical | Excuse Generation Unit | Demo Experience Unit | Include |
| US-007 History Recording | MVP Core | History and Persistence Unit | Demo Experience Unit | Include |
| US-008 Estimation Fallback | MVP Core | Workload Estimation Unit | Demo Experience Unit | Include |
| US-009 Deadline Failure Flow | Later | Excuse Generation Unit | History and Persistence Unit, later Failure Recovery Unit | Later |

## Unit-to-Story Map

### Demo Experience Unit

**Primary Stories**
- US-001: Minimal Task Intake

**Supporting Stories**
- US-002, US-003, US-004, US-005, US-006, US-007, US-008

**Demo Role**: 入力から結果表示までをつなぎ、各能力を「人をダメにする一連の体験」として見せる。

### Workload Estimation Unit

**Primary Stories**
- US-002: LLM Workload Estimation
- US-008: Estimation Fallback

**Supporting Stories**
- US-003: Latest Start Decision
- US-007: History Recording

**Demo Role**: ユーザーが見積もりから逃げても、アプリが不確実性つきで作業量を出す。

### Deadline Decision Unit

**Primary Stories**
- US-003: Latest Start Decision
- US-005: Risk Band Explanation

**Supporting Stories**
- US-004: Guilt-Free Idle Time

**Demo Role**: 「いつまで始めなくてよいか」を確率とリスク帯で示す中核体験を作る。

### Idle Planning Unit

**Primary Stories**
- US-004: Guilt-Free Idle Time

**Supporting Stories**
- US-005: Risk Band Explanation

**Demo Role**: サボってよい時間を、娯楽・休憩・現実逃避プランに変換し、テーマ適合性を強くする。

### Excuse Generation Unit

**Primary Stories**
- US-006: Excuse Generation

**Later Stories**
- US-009: Deadline Failure Flow

**Demo Role**: 失敗や危険状態に対して、反省より先に言い訳の下書きを提示する。

### History and Persistence Unit

**Primary Stories**
- US-007: History Recording

**Supporting Stories**
- US-002: LLM Workload Estimation
- US-008: Estimation Fallback
- US-009: Deadline Failure Flow, later

**Demo Role**: 過去の先延ばし傾向をアプリに解釈させ、次回の判断放棄を支える。

## Acceptance Criteria Traceability

| Acceptance Criterion | Covered By Unit |
|---|---|
| AC-001 Minimal Input Works | Demo Experience Unit |
| AC-002 Latest Start Time Is Shown | Deadline Decision Unit |
| AC-003 Risk Is Not Asserted as Certainty | Deadline Decision Unit, Workload Estimation Unit |
| AC-004 LLM Failure Does Not Break Demo | Workload Estimation Unit |
| AC-005 Guilt-Free Idle Time Is Visible | Idle Planning Unit |
| AC-006 Excuse Generation Exists | Excuse Generation Unit |
| AC-007 History Is Stored | History and Persistence Unit |
| AC-008 Theme Fit Is Explicit | All MVP Units |

## Human-Degrading Effect Coverage

| Effect | Unit |
|---|---|
| 入力の面倒から逃げる | Demo Experience Unit |
| 見積もり判断を放棄する | Workload Estimation Unit |
| 着手判断を放棄する | Deadline Decision Unit |
| 罪悪感なくサボる | Idle Planning Unit |
| 現実逃避先まで委ねる | Idle Planning Unit |
| 反省より先に言い訳を作る | Excuse Generation Unit |
| 自己分析を履歴に任せる | History and Persistence Unit |

## Business Value Coverage

| Business Value | Unit |
|---|---|
| MVP デモの一貫した体験 | Demo Experience Unit |
| 外部 AI 依存を下げる実装可能性 | Workload Estimation Unit |
| サービス Intent の中核 | Deadline Decision Unit |
| テーマ性とデモ映え | Idle Planning Unit |
| 創造性と記憶に残る差別化 | Excuse Generation Unit |
| 継続利用と AWS DB 拡張 | History and Persistence Unit |

## PBT Candidate Traceability

| PBT Candidate | Unit | Later Construction Focus |
|---|---|---|
| Latest start is not after deadline | Deadline Decision Unit | Functional Design, Code Generation |
| Success probability remains 0..100 | Deadline Decision Unit | Functional Design, Code Generation |
| Risk band classification follows thresholds | Deadline Decision Unit | Functional Design, Code Generation |
| Idle minutes are never negative | Idle Planning Unit | Functional Design, Code Generation |
| Idle minutes decrease as latest start approaches | Idle Planning Unit | Functional Design, Code Generation |
| Fallback estimate returns valid estimate | Workload Estimation Unit | Functional Design, Code Generation |
| Confidence remains 0..100 | Workload Estimation Unit | Functional Design, Code Generation |

## MVP Completeness Check

| Check | Result |
|---|---|
| Every MVP story maps to at least one Unit | Pass |
| Every MVP component maps to a Unit | Pass |
| Every Unit includes human-degrading effect | Pass |
| Every Unit includes business value | Pass |
| Screens, APIs, and data targets are identified | Pass |
| MVP vs Later split is explicit | Pass |
| PBT candidates are carried forward | Pass |

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | Story mapping carries PBT candidates forward. PBT enforcement applies in later Construction artifacts. |
