# Requirements Verification Questions

このファイルに回答を記入してください。各質問の `[Answer]:` の後に、選択肢の文字を入力してください。選択肢に合わない場合は最後の `Other` を選び、同じ行または次の行に補足を書いてください。

## Context

対象サービスは、締め切りギリギリまで着手しない人に対して「まだ始めなくてよい時間」を推定し、罪悪感を減らしながら先延ばしできる体験を提供するWebアプリです。

Requirements Analysisでは、普通のタスク管理アプリに寄せすぎず、最終的に動くMVPとして成立する範囲を決めます。

## Question 1
このサービスの最重要Intentはどれにしますか？

A) 先延ばしを肯定しつつ、破綻しないギリギリの開始判断を支援する
B) 罪悪感を減らし、ユーザーが休憩や現実逃避を楽しめる時間を作る
C) 締め切り失敗時の言い訳や反省回避まで含めた体験を提供する
D) ハッカソンテーマへのインパクトを最大化する
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 2
MVPで必ず成立させたい中心体験はどれですか？

A) タスク入力後、最遅着手時刻とリスク帯がすぐ表示される体験
B) 「サボってよい時間」と現実逃避プランが楽しく提示される体験
C) 履歴から自分の先延ばし傾向が反映される体験
D) 間に合わなかった時の言い訳生成まで含む体験
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 3
最遅着手時刻の推定は、MVPではどの程度の精度・説明性を目指しますか？

A) ルールベースでよい。入力値から安全圏・危険圏を説明可能に算出する
B) 簡易スコアリングでよい。作業量、重要度、過去成功率から成功確率を出す
C) 履歴学習を重視する。使うほど個人傾向が反映されることを見せる
D) AI推定らしさを重視する。ただし断定せず確率とリスクで表現する
X) Other (please describe after [Answer]: tag below)

[Answer]: X LLMによってタスク内容と過去DBから作業時間を推定する

## Question 4
ユーザーに入力させるタスク情報として、MVPで必須にしたい範囲はどれですか？

A) タスク名、締め切り、見積もり作業時間のみ
B) Aに加えて、重要度、難易度、集中しやすい時間帯
C) Bに加えて、過去の類似タスク、失敗時の影響、言い訳しやすさ
D) できるだけ入力を少なくし、初回デモで流れが速く見えることを優先する
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 5
「人をダメにする」表現のトーンはどこまで攻めますか？

A) 軽いユーモア中心。実用性を崩さない
B) 皮肉や背徳感を強めるが、ユーザーを責めない
C) かなり攻める。サボり、言い訳、反省回避を前面に出す
D) デモや審査資料では攻め、アプリ本体では実用寄りに調整する
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 6
言い訳生成はMVPに含めますか？

A) 含める。テーマ適合性とデモ映えを優先する
B) 条件付きで含める。失敗時または危険圏のみ表示する
C) 後続拡張に回す。MVPは最遅着手とサボり時間に集中する
D) 含めるが、実害を避けるため「下書き」「ユーモア」扱いにする
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 7
履歴データはMVPでどこまで扱いますか？

A) ブラウザ内保存で十分。ログインなしで達成・失敗履歴を残す
B) 簡易DBを使う。後でAWSデプロイしやすい構成にする
C) MVPでは履歴なし。サンプル履歴でデモし、後続で実装する
D) 履歴入力だけ用意し、推定への反映は限定的にする
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 8
MVPの想定プラットフォームとして最も近いものはどれですか？

A) フロントエンド中心のWebアプリ。ローカル保存で完結
B) Webフロントエンド + 軽量API + 簡易DB
C) AWSデプロイ前提のサーバーレスWebアプリ
D) まずは静的デモで体験を完成させ、後でAPI/DBを追加する
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 9
予測・推定の安全な見せ方として、どれを重視しますか？

A) 成功確率、リスク帯、安全圏/危険圏を明示し、断定を避ける
B) 「まだサボれるが、この条件なら危険」の説明を厚くする
C) 失敗可能性や見積もり不確実性を常に表示する
D) ユーモアを保ちつつ、締め切り破綻を助長しすぎないガードを入れる
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 10
User Storiesで特に深掘りしたい心理はどれですか？

A) 先延ばしと罪悪感の軽減
B) 判断放棄と「アプリに決めてもらう」感覚
C) 言い訳、反省回避、責任の外部化
D) サボる時間を娯楽として正当化する感覚
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 11
Application Designで独自コンポーネントとして強調したいものはどれですか？

A) Procrastination Deadline Engine: 最遅着手時刻とリスク帯を算出する
B) Guilt-Free Idle Planner: サボってよい時間と現実逃避プランを提案する
C) Excuse Generator: 失敗時の言い訳文を生成する
D) Self-Deception History: 過去の成功・失敗・言い訳傾向を蓄積する
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Question 12
Unit分解では、どの観点を最優先にしますか？

A) 「人をダメにする能力」単位を最優先し、技術単位は各Unit内に含める
B) ビジネス能力単位を最優先し、責務・価値・画面/API/データを対応づける
C) MVP実装しやすさを最優先し、後から能力単位に説明を整理する
D) 審査資料のわかりやすさを最優先し、能力単位と実装単位を併記する
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 13
初期MVPのスコープ外にしてよいものはどれですか？

A) 外部カレンダー連携
B) 本格的なユーザー認証
C) 外部AI APIへの強い依存
D) チーム共有・共同タスク管理
X) Other (please describe after [Answer]: tag below)

[Answer]: X　A, B, C, Dすべて初期MVPスコープ外

## Question 14
Security Baseline拡張ルールをこのプロジェクトで有効にしますか？

A) Yes - SECURITYルールをブロッキング制約として適用する
B) No - PoC/プロトタイプとしてSECURITYルールはスキップする
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 15
Property-Based Testing拡張ルールをこのプロジェクトで有効にしますか？

A) Yes - PBTルールをブロッキング制約として適用する
B) Partial - 純粋関数とシリアライズ往復など限定範囲に適用する
C) No - MVPでは通常の単体テスト中心にする
X) Other (please describe after [Answer]: tag below)

[Answer]: B
