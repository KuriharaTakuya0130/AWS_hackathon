# Idle Entertainment Clarification Questions

Application Designで「サボってよい時間に合った娯楽・休憩・現実逃避プラン提案」を明文化するための追加質問です。

現在の設計では `Guilt-Free Idle Planner` は存在しますが、主に「サボってよい時間の計算」に寄っています。以下の回答をもとに、`components.md`、`component-methods.md`、`services.md`、`component-dependency.md`、`application-design.md` へ反映します。

## Question 1
娯楽提案機能の正式な位置づけはどれにしますか？

A) `Guilt-Free Idle Planner` の責務として含める
B) `Idle Entertainment Recommender` として独立コンポーネントにする
C) `Guilt-Free Idle Planner` の内部サブ機能として扱い、Units Generationでは独立Unit候補にもできるようにする
D) MVPでは表示文言のみで扱い、後続拡張に回す
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 2
MVPで提案する娯楽・休憩・現実逃避プランの粒度はどれにしますか？

A) 時間枠ごとの固定テンプレートを出す
B) タスク内容、残り時間、リスク帯に応じた簡易ルールで出す
C) LLMで娯楽プランを生成し、失敗時はテンプレートへフォールバックする
D) ユーザーの履歴や好みまで反映する
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 3
提案カテゴリはどれをMVPに含めますか？

A) 短時間休憩のみ
B) 休憩、娯楽、現実逃避の3カテゴリ
C) 安全圏では娯楽/現実逃避、危険圏では短時間休憩に制限する
D) 休憩、娯楽、現実逃避、言い訳準備の4カテゴリ
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 4
リスク帯と娯楽提案の関係はどれにしますか？

A) 安全圏、危険圏、詰みかけに関係なく楽しい提案を出す
B) 安全圏は積極的にサボり提案、危険圏は軽い休憩、詰みかけは言い訳生成へ誘導する
C) 危険圏以上では娯楽提案を止め、作業開始を促す
D) リスク帯ごとに提案トーンだけ変える
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 5
娯楽提案の「人をダメにする効果」はどれを最重要にしますか？

A) サボる時間を正当化する
B) 罪悪感を減らして娯楽に没入させる
C) 現実逃避の選択までアプリに委ねさせる
D) 危険圏まで反省を先送りさせる
X) Other (please describe after [Answer]: tag below)

[Answer]: C

## Question 6
娯楽提案の出力には何を含めますか？

A) プラン名と推奨時間のみ
B) プラン名、推奨時間、戻るべき時刻
C) Bに加えて、なぜ今それをしてよいのかの免罪符メッセージ
D) Cに加えて、危険になったら切り上げる条件
X) Other (please describe after [Answer]: tag below)

[Answer]: B

## Question 7
API/メソッドとしてはどの形で明文化しますか？

A) `GuiltFreeIdlePlanner.suggestIdleActivities(...)` を追加する
B) `IdleEntertainmentRecommender.recommend(...)` を追加する
C) BFF APIの分析結果に `idleActivitySuggestions` を含める
D) AとCの両方を追加する
X) Other (please describe after [Answer]: tag below)

[Answer]: D

## Question 8
Units Generationでは娯楽提案をどの扱いにしますか？

A) `Idle Planning Unit` の一部にする
B) `Idle Entertainment Recommendation Unit` として独立候補にする
C) MVPでは `Idle Planning Unit` に含め、後続拡張で独立Unit化できるよう記載する
D) Units Generationでは扱わず、UIの表示項目として扱う
X) Other (please describe after [Answer]: tag below)

[Answer]: C
