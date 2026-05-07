# Personas

## Persona Strategy

User Storiesでは、3名のペルソナを定義する。

最重要ペルソナは、**サボる時間を正当化して楽しみたい人**である。この選択により、サービスの中心価値を「早く始める支援」ではなく、「サボる許可を与える体験」として明確にする。

## Persona 1: サボり時間を正当化したい人

### Profile
- **Name**: 佐伯 ミナ
- **Role**: 大学生 / 個人利用者
- **Situation**: レポート、課題、発表準備をいつも締め切り直前まで引き延ばす。
- **Primary Type**: サボる時間を正当化して楽しみたい人

### Goals
- まだ始めなくてよいなら、安心して娯楽に時間を使いたい。
- 「今やらない自分」を責めずに済む根拠がほしい。
- どうせギリギリになるなら、破綻しない範囲でサボりたい。

### Frustrations
- タスク管理アプリを見ると、早く始めろと言われているようで嫌になる。
- 締め切りが近いのに、まだ大丈夫かどうかを自分で判断できない。
- サボっている間も、頭の片隅でタスクが気になり続ける。

### Degeneration Effect
- アプリが「まだサボってよい」と言うことで、サボりを自己正当化できる。
- 自分で着手判断をしなくなり、判断をアプリへ預ける。
- サボる時間を罪悪感ではなく、許可された余白として楽しむ。

### User Value
- 先延ばし中の不安と罪悪感が減る。
- いつまでサボれるかを視覚的に理解できる。
- 危険圏に入った時点で現実へ戻るきっかけを得られる。

### Business Value
- サービスのテーマ適合性を最も強く体現する中心ユーザー。
- デモで「人をダメにするサービス」の体験が一目で伝わる。
- サボり時間可視化、現実逃避プラン、言い訳生成の利用動機が強い。

### Relevant Stories
- US-001: Minimal Task Intake
- US-002: LLM Workload Estimation
- US-003: Latest Start Decision
- US-004: Guilt-Free Idle Time
- US-006: Excuse Generation

## Persona 2: 見積もりが甘く毎回焦る人

### Profile
- **Name**: 藤野 ケイ
- **Role**: 若手ビジネスパーソン / 個人利用者
- **Situation**: 資料作成や申請作業を軽く見積もり、毎回最後に焦る。
- **Primary Type**: 見積もりが甘く、毎回ギリギリで焦る人

### Goals
- タスクが実際にどれくらい重いのかを早めに知りたい。
- まだ大丈夫なのか、もう危ないのかを数字で見たい。
- 自分の過去の失敗傾向を次回の判断に活かしたい。

### Frustrations
- 自分の見積もりを信用できない。
- 締め切りまでの残り時間だけでは危険度がわからない。
- 生産性アプリは理想論が多く、実際の先延ばし癖に寄り添わない。

### Degeneration Effect
- 自分の見積もりを放棄し、LLMと履歴に判断を任せる。
- アプリの成功確率を根拠に、開始判断を先送りする。
- 危険圏までは自分で反省しない。

### User Value
- 作業時間の推定とリスク帯により、破綻ラインを把握できる。
- LLMが失敗してもフォールバック推定で判断を継続できる。
- 履歴により、自分の甘い見積もりを補正できる。

### Business Value
- 推定エンジン、履歴DB、PBT対象ロジックの必要性を説明しやすい。
- 実用性とテーマ性のバランスを示すペルソナ。
- 後続の有料化や継続利用価値に接続しやすい。

### Relevant Stories
- US-002: LLM Workload Estimation
- US-003: Latest Start Decision
- US-005: Risk Band Explanation
- US-007: History Recording
- US-008: Estimation Fallback

## Persona 3: 言い訳で心理的に逃げたい人

### Profile
- **Name**: 久保 レン
- **Role**: フリーランス / 個人利用者
- **Situation**: 締め切り遅延時に、相手へどう説明するかでさらに時間を失う。
- **Primary Type**: 言い訳を先に考えて安心したい人

### Goals
- 間に合わなかった時に使える言い訳の下書きがほしい。
- 失敗時に完全に詰んだ気分にならず、心理的な逃げ道を確保したい。
- 言い訳を考える負担を減らしたい。

### Frustrations
- 締め切りに遅れた後、説明文を書くのがつらい。
- 反省する前に防御的な文章を考えてしまう。
- 言い訳が雑だと、さらに状況が悪くなる。

### Degeneration Effect
- 反省より先に言い訳を生成し、責任の一部を外部化する。
- 失敗時の心理的逃げ道があることで、ギリギリまで粘りやすくなる。
- 先延ばし後の後始末までアプリに頼る。

### User Value
- 失敗時の心理負荷を下げる。
- 遅延連絡の文章を短時間で作れる。
- ユーモアとしてもデモ体験としても伝わりやすい。

### Business Value
- ハッカソンテーマへの創造性とデモ映えを強化する。
- ただの推定アプリではない独自性を示す。
- 言い訳生成Unitとして分解しやすい。

### Relevant Stories
- US-006: Excuse Generation
- US-005: Risk Band Explanation
- US-009: Deadline Failure Flow

## Persona-to-Capability Map

| Persona | Primary Capability | Supporting Capabilities |
|---|---|---|
| サボり時間を正当化したい人 | Guilt-Free Idle Planning | Latest Start Decision, Excuse Generation |
| 見積もりが甘く毎回焦る人 | Procrastination Deadline Engine | LLM Workload Estimation, Self-Deception History |
| 言い訳で心理的に逃げたい人 | Excuse Generation | Risk Band Explanation, Deadline Failure Flow |

## Persona Coverage

| Story | サボり時間を正当化したい人 | 見積もりが甘く毎回焦る人 | 言い訳で心理的に逃げたい人 |
|---|---:|---:|---:|
| US-001 | Yes | Yes | No |
| US-002 | Yes | Yes | No |
| US-003 | Yes | Yes | No |
| US-004 | Yes | No | No |
| US-005 | Yes | Yes | Yes |
| US-006 | Yes | No | Yes |
| US-007 | No | Yes | No |
| US-008 | No | Yes | No |
| US-009 | No | No | Yes |

## INVEST Persona Support

- ペルソナは実装対象の画面やAPIを固定せず、ユーザー価値と心理体験を中心に定義している。
- 3名に限定することで、MVPに対して過剰なストーリー量にならない。
- 各ペルソナはUser StoriesとCapability/Unit候補に接続できる。

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | Personas artifact does not define design, code, or test implementation subject to PBT enforcement. |
