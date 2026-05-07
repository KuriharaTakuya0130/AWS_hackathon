# Idle Entertainment Follow-up Questions

娯楽提案機能の回答に1点だけ設計上の確認があります。

- Question 1では、娯楽提案機能を `Idle Entertainment Recommender` として独立コンポーネントにする回答でした。
- Question 7では、`GuiltFreeIdlePlanner.suggestIdleActivities(...)` とBFF API出力の両方を追加する回答でした。

独立コンポーネントにする場合、主要メソッドを `IdleEntertainmentRecommender.recommend(...)` に置くのか、`GuiltFreeIdlePlanner.suggestIdleActivities(...)` に置くのかを明確にする必要があります。

## Question 1
娯楽提案機能の正式なメソッド配置はどれにしますか？

A) `IdleEntertainmentRecommender.recommend(...)` を正式メソッドにし、BFF API出力に `idleActivitySuggestions` を含める
B) `GuiltFreeIdlePlanner.suggestIdleActivities(...)` を正式メソッドにし、独立コンポーネント化はしない
C) `IdleEntertainmentRecommender.recommend(...)` を正式メソッドにし、`GuiltFreeIdlePlanner` はその結果をIdle Planに統合する
D) MVPでは `GuiltFreeIdlePlanner.suggestIdleActivities(...)`、後続拡張で `IdleEntertainmentRecommender` へ分離する
X) Other (please describe after [Answer]: tag below)

[Answer]: C
