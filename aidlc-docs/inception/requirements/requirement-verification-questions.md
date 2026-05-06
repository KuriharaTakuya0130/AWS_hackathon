# Requirements Verification Questions

以下の質問は、ハッカソン提出用サービスのRequirements Analysisを完了するために必要です。
各質問の `[Answer]:` の後に、該当する選択肢の記号を記入してください。
選択肢に合わない場合は、最後の `X) Other` を選び、同じ `[Answer]:` 行の後に内容を記述してください。

## Question 1
このサービスの最も重要なIntentはどれですか？

A) ユーザーが「いつまで始めなくてよいか」を判断でき、罪悪感なく先延ばしできる状態を作る
B) ユーザーが締め切りに間に合うよう、最適なタスク管理と作業計画を支援する
C) ユーザーの娯楽時間を最大化し、タスクの成否は副次的に扱う
D) ユーザーが失敗したときの言い訳や自尊心保護を主目的にする
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 2
このサービスが「人をダメにする」中核は、どの人間能力を最も強く手放させることですか？

A) 着手判断: いつ始めるべきかを自分で決めない
B) 自制心: サボりたい欲求を我慢しない
C) 計画力: 作業計画を自分で立てない
D) 反省と責任感: 失敗時の解釈や言い訳を委ねる
E) 複合型: AからDを組み合わせる
X) Other (please describe after [Answer]: tag below)

[Answer]: E

## Question 3
主な対象ユーザーは誰にしますか？

A) 課題やレポートを締め切り直前まで先延ばしする学生
B) 仕事のタスクを締め切り直前まで着手しない社会人
C) 趣味・家事・個人タスクを後回しにしがちな一般ユーザー
D) 学生と社会人の両方を対象にする
X) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 4
初期版で扱うタスクの範囲はどれにしますか？

A) 締め切りが明確な単発タスクに絞る
B) 締め切りが明確なタスクに加え、締め切りが曖昧なタスクも扱う
C) 学校課題・仕事・家事など幅広いタスクを最初から扱う
D) ハッカソン提出では概念設計を広く示し、初期実装スコープは単発タスクに絞る
X) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 5
「最も遅い着手時刻」の提示は、どの表現にしますか？

A) 安全圏・危険圏・破滅圏の3段階で表示する
B) 成功確率付きの時刻として表示する
C) 推奨時刻とリスク説明を組み合わせて表示する
D) 断定を避け、複数の選択肢として表示する
X) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 6
初期実装での見積もり精度は、どの方針にしますか？

A) ルールベースの簡易見積もりから始め、利用履歴で補正する
B) LLMにタスク内容を解釈させ、作業時間やリスクを推定する
C) ユーザー入力を中心にし、AIは説明や提案文生成に使う
D) ルールベースとLLMを組み合わせる
X) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 7
サボり時間に提案する内容は、どこまで含めますか？

A) アプリ内で完結する短時間娯楽・休憩・現実逃避プランの提案
B) 飲食・動画・ゲームなど外部サービスも想定するが、初期実装では連携しない
C) 外部API連携や決済まで含めた将来構想として設計する
D) 娯楽提案よりも、先延ばし正当化メッセージを中心にする
X) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 8
言い訳生成機能は、Requirements上どの位置づけにしますか？

A) 初期版の主要機能に含める
B) テーマ適合性を高める補助機能として含める
C) 将来拡張として扱い、初期版では扱わない
D) 倫理・印象リスクがあるため、言い訳ではなくリカバリー説明に寄せる
X) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 9
ビジネス価値は、どの方向を主軸にしますか？

A) 先延ばし癖を否定せず、破綻しない範囲でサボりと作業を調停する個人向けサブスク
B) ユーザーの空き時間・サボり時間に娯楽や飲食を推薦する送客・広告基盤
C) 学生・社会人向けのユーモアあるタスク支援アプリとしてプレミアム機能課金する
D) BとCを組み合わせ、短期はプレミアム課金、将来は推薦基盤に広げる
X) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 10
ハッカソン提出物として、どのInception成果物を重視しますか？

A) Requirements AnalysisとUnit of Work計画を最優先する
B) Requirements Analysis、User Stories、Application Design、Units Generationをすべて重視する
C) 創造性を示すため、User StoriesとApplication Designを特に厚くする
D) Unit分解の評価を狙い、Units Generationを最も厚くする
X) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 11
Should security extension rules be enforced for this project?

A) Yes - enforce all SECURITY rules as blocking constraints (recommended for production-grade applications)
B) No - skip all SECURITY rules (suitable for PoCs, prototypes, and experimental projects)
X) Other (please describe after [Answer]: tag below)

[Answer]: 

## Question 12
Should property-based testing (PBT) rules be enforced for this project?

A) Yes - enforce all PBT rules as blocking constraints (recommended for projects with business logic, data transformations, serialization, or stateful components)
B) Partial - enforce PBT rules only for pure functions and serialization round-trips (suitable for projects with limited algorithmic complexity)
C) No - skip all PBT rules (suitable for simple CRUD applications, UI-only projects, or thin integration layers with no significant business logic)
X) Other (please describe after [Answer]: tag below)

[Answer]: 

