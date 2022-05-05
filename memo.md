# メモ
## 5/5
- bert-for-patentというのがあるらしい
    - タスクに完全に一致しているのから使ってみる
    - https://huggingface.co/anferico/bert-for-patents

- seedによってはkfoldで極端にスコアが異なる
    - 分ける時に学習してない単語などが入ってしまう？
    - anchorで


# 実験管理
## exp001
- model:roberta-base
- kfold, 5fold
- MSELoss
- contextは分割して数値化
- cv:0.80,lb:0.765

## exp003
- model:deverta-v3-large
- kfold, 5fold
- MSELoss
- nakamaさんのnotebookからcontextの処理を借りた
- cv:0.867, lb: