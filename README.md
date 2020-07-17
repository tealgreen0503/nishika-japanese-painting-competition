# 日本絵画に描かれた人物の顔分類に機械学習で挑戦！ 2nd place solution
- https://www.nishika.com/competitions/5/

## モデル概要
- pretrained models
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
 
## 最終順位
### Public Score
- 1位


### Private Score
- 2位
