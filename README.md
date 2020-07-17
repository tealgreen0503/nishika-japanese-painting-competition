# 日本絵画に描かれた人物の顔分類に機械学習で挑戦！ 2nd place solution

## 動作環境
- Google Colaboratory上での動作を前提
  - ただしGPUが"Tesla P100-PCIE-16GB"でないとCUDA out of momoryとなる

## 実行手順
- "training_model_1.ipynb" ~ "training_model_3.ipynb" を実行し，"inference_and_ensemble.ipynb"を実行
  - 学習済みモデルは全て同じ場所に保存されるため，一つのノートブックを実行するたびに学習した重みを別の場所に退避させる
- outputディレクトリに"submission_emsemble_tta.csv"が作成される

## モデル概要
- 学習済みモデルとして以下を使用
  - EfficientNet B7 (seed=42, https://github.com/lukemelas/EfficientNet-PyTorch)
  - EfficientNet B7 Noisy Student (seed=43, https://github.com/rwightman/pytorch-image-models)
  - EfficientNet B8 (seed=44, https://github.com/rwightman/pytorch-image-models)
- image size
  - (256,256)
- DA
  - HorizontalFlip
  - RandAugument (N=2, M=15) (ttps://github.com/rwightman/pytorch-image-models)
- CV
  - 5fold(StratifiedKFold)
- epoch
  - 30
- optimizer
  - SGD
    - lr
      - 最後の全結合層: 1e-1
      - それ以外　　　: 1e-2
    - weight_decay: 1e-4
    - momentum: 0.9
- scheduler
  - 21エポック以降は学習率を0.5倍
- criterion
  - CrossEntropyLoss
- TTA
  - w/o HorizontalFlip
  - w/ HorizontalFlip
- ensemble
  - モデル3種とTTA2パターンの合計6つのモデルでアンサンブル
  - 出力の単純平均をとる
