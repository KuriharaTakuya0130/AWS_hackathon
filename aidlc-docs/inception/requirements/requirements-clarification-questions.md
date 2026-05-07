# Requirements Clarification Questions

回答内容の大部分はRequirements Analysisに進められる状態です。ただし、以下の2点だけ曖昧さがあります。

- Question 3では「LLMによってタスク内容と過去DBから作業時間を推定する」と回答されています。
- Question 13では「外部AI APIへの強い依存」を初期MVPスコープ外にしています。
- Question 4では「入力をできるだけ少なくする」と回答されています。

このままだと、MVPで外部LLMに依存するのか、内部ロジック中心にするのか、推定に必要な最低入力をどう確保するのかが曖昧になります。以下に回答してください。

## Clarification Question 1
MVPにおけるLLM利用の位置づけはどれにしますか？

A) LLMはMVPの必須機能にする。ただし依存を弱めるため、失敗時はルールベース推定にフォールバックする
B) LLMは任意機能にする。MVPの中核推定はルール/スコアリングで成立させ、LLMは作業時間見積もり補助に限定する
C) LLMはMVPでは使わない。LLM風の説明文やサンプル推定でデモし、後続拡張に回す
D) LLMは言い訳生成や娯楽プラン生成に使い、最遅着手時刻の算出には使わない
X) Other (please describe after [Answer]: tag below)

[Answer]: A

## Clarification Question 2
入力を少なくしつつ推定の説得力を保つため、MVPで必須入力にする項目はどれにしますか？

A) タスク名、締め切りのみ。作業時間はLLM/履歴/デフォルト値で推定する
B) タスク名、締め切り、ユーザーのざっくり作業量だけ必須にする
C) タスク名、締め切り、作業量、難易度だけ必須にする
D) デモ用は最小入力、本番想定は詳細入力という二段階入力にする
X) Other (please describe after [Answer]: tag below)

[Answer]: A
