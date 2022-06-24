# メモ
## 5/5
- bert-for-patentというのがあるらしい
    - タスクに完全に一致しているのから使ってみる
    - https://huggingface.co/anferico/bert-for-patents

- seedによってはkfoldで極端にスコアが異なる
    - 分ける時に学習してない単語などが入ってしまう？→どうやら学習率が高すぎるだけだった」
    - anchorでsplitしよう
- lossを改良できないか
    - 損失関数は外れ値に引っ張られるので、huber損失が使えないか？
    - 分散を考慮した損失関数を作りたい
    - クラス分類問題にしてkl divergenceも使えそう
- nn層
    - multi dropoutが効くらしい
    - https://www.kaggle.com/code/santhoshkumarv/patentphrase-roberta-inference-lb-0-814
- モデル候補
    - bert-for-patent:https://huggingface.co/anferico/bert-for-patents
    - deberta-v3-large:https://huggingface.co/microsoft/deberta-v3-large
    - deberta-v2-xlarge:https://huggingface.co/microsoft/deberta-v2-xlarge
    - roberta-large:https://huggingface.co/roberta-large
    - cocolm-large:https://huggingface.co/microsoft/cocolm-large
    - patentsberta:https://huggingface.co/AI-Growth-Lab/PatentSBERTa
    - funnel-transformer:https://huggingface.co/funnel-transformer/xlarge
- 初期化
    - 層を初期化してしまうのが良かったらしい
    - https://www.ai-shift.co.jp/techblog/2145



# 実験管理


## baseline
### deberta-v3-large:cv:0.82968 lb:0.8376
- deberta-v3-large
- 5fold
- 5epoch


## exp001：cv:0.80,lb:0.765
- model:roberta-base
- kfold, 5fold
- MSELoss
- contextは分割して数値化

## exp003：cv:0.867, lb:0.837
- model:deberta-v3-large
- kfold, 5fold
- MSELoss
- nakamaさんのnotebookからcontextの処理を借りた
- 層化抽出した方が良さげ

## exp004:cv:0.8236, lb:0.8393
- exp003のfold→group:anchor,stratified:score

## exp005:cv:0.8119, lb:0.8282
- bert-for-patentを使用

## exp004とexp005のアンサンブル:lb:0.8453

## exp006:cv:0.8244, lb:0.8390
- deberta-v3-large
- exp004のlossをhuberlossに変更

## exp007:cv:0.8297, lb:0.8376
- exp006でepoch5にしたら過学習気味だったのでepoch4にする

## exp008:cv:, lb:
- bert-for-patent
- huberlossを適用する
- 学習がうまくいかなかった

## exp009:cv:0.8131, lb:0.8123
- deberta-v3-large
- exp004のfoldを7つに変えた
- よくないっぽい

## exp010:cv:0.824, lb:0.837
- deberta-v3-large
- debertaのdropoutを0にする

## exp011:cv:0.8276, lb:0.8372
- deberta-v3-large
- cnnをheadにつける

## exp012:cv:0.82564, lb:0.8347
- deberta-v3-large
- 最終4層のclsトークンをとってつなげる

## exp012,exp004, exp006, exp005のアンサンブル:めっちゃ下がった

## exp013:cv:0.8325, lb:0.8370
- deberta-v3-large
- foldを5→7,epochを3に変更

## exp014:cv:0.8668, lb:
- stratified:scoresだけにした


===============ここからseed固定できてないのに気づいた
## exp015:cv:0.83068, lb:0.8337
- deberta-v3-large
- stratified and grouped
- 最終4層を使用する

## exp016:cv:0.82866, lb:0.8362
- deberta-v3-large
- stratified and grouped
- 畳み込みを使用する

## exp017:cv:0.82696, lb:0.8334
- deberta-v3-large
- batch-sizeを16→8に
- stratified and grouped
- mixoutを使ってみる

## exp018:cv:0.8303, lb:0.8376
- cpc_textの処理にバグがあったようなので治した
- discussionではバグがあった方がオーギュメンテーションみたいになってスコア高いって書いてある
-

## exp019:cv:0.82563, lb:
- deberta-v3-large
- 4層を初期化してtrainする

## exp020:cv:0.7962, lb:0.824
- roberta-large

## exp021:cv:0.78785, lb:0.7964
- patentBERTa
- 

## exp022:cv:0.82334, lb:0.8363
- funnel-large

## exp023：cv:, lb
- albert-v2-xxlarge
- 

## exp024：cv:, lb
- roberta-large
- attentionをつけてみる
- 

## balseline_ernie:cv:0.8006, lb:0.8206

## exp025:cv:0.82807, lb:0.8380
- deberta-v3-large
- layernormを使用

## exp026:cv:0.8303, lb:0.8355
- deberta-v3-large
- 最終層でトークンの平均を取る
- 

## exp027:cv:, lb:
- muppet-roberta-large
- 

## exp028:cv:, lb:
- deberta-v3-large
- nnの構造はそのままで、layernorm加える
- 

## exp029:cv:0.825, lb:0.8283
- electra-large

## exp030:cv:0.8259, lb:0.8373
- funnel-large
- decay修正

## exp031:cv:0.8271, lb:0.8313
- deberta-v3-large
- decay直してattentionつけた

## exp032:cv:0.8158, lb:0.8269
- bert-for-patents
- decayの修正

## exp033:cv:0.830, lb:0.8385
- deberta-v3-large
- decayだけ直して実験

## exp034:cv:0.79602, lb:
- roberta-large

## exp035:cv:0.7973, lb:0.8167
- luke-large
- 

## exp036:cv:, lb:

## exp037:cv:, lb:
-  deberta-v3-small
-  

## アンサンブル
### exp004&exp005
- cv:0.8444, lb:0.8453

### exp005&exp018&exp020&exp022
- cv:0.8469, lb:0.8504
- 

### 