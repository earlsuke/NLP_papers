[title](URL)

## 概要/どんなもの?

### モチベーション
- DNNとNLPはサーバー上のハードウェアで動くことを想定して開発されてきた
- セキュリティ、ネットワーク遅延、運用コスト削減のために、オンデバイスモデルにも需要がある
- モバイルで動くようなモデルはパフォーマンスを保ったまま、小さく効率的である必要性がある

### PRADOの提案
- 200k未満のパラメータモデル
    - タスクに関連する特徴のみを学習するから、小さく作ることができた

- SGNNとの相違点
    - static projectionsを持つSGNNと異なり、attentionとconvolutionを使って、より長い距離の依存関係を扱える学習可能なprojectionsを持つから長い文書分類に向いている

- 量子化してアーキテクチャの軽量化にも取り組んだ

### PRADOによる成果
    - on-deviceで動作する、長い文書分類のためのアテンションと畳込みを組み合わせた転移可能な射影
    - 複数の長い文書分類タスクに対しての徹底的な実験と伝統的な特徴量エンジニアリングを駆使した線形分類器とDL手法をout-perform
    - 量子化されたPRADOは200kでCNNとLSTMをout-perform
    - 限定データ環境下でのパフォーマンス改善における転移学習の適用可能性

### pQRNNという拡張を紹介
- 最小モデルサイズでNLPを改善するテク
- 新しさは、高速な並列処理のために以下を利用する点
    - 射影の操作
    - qRNNエンコーダー
- 結果、BERTと同等レベルのパフォーマンスを発揮

### PRADOについて
    - 1年前の開発時点では、テキストセグメンテーションに関するドメイン知識を使ってモデル縮小、パフォーマンスを図った
    - テキストは語彙に対応する表現として変換される
    - このセグメント化は、モデルパフォーマンス、サイズ、処理時間に影響を与える
    - 言語モデリングやMTなどの少数タスクでは、セグメント間の差異に性能が依存する
    - 大多数の問題では、タスクに関連したセグメントのサブセットのみを使って問題を解ける
        - e.g. sentiment classificationでは、感情に関連するセグメントのみを見ることでOK
    - 単語の断片や文字ではなくセグメントのクラスタを学習
    - 複雑度の低いタスクで優位なスコアを収めた

### PRADOの改善
    - pQRNNの構成要素
        - トークンのprojection
            - テキスト内のtokenを3次元の特徴ベクトルに変換(keyを作るイメージ)
        - denseなbottleneck
            - トークン依存の情報を保存(value:埋め込み先を作る)
        - QRNNエンコーダー
            - トークン同士の双方向依存関係を保存
### モデル構成
- projected embedding layer
    - NLP向けのDNNは埋め込み層Wが大きい
    - W = |V| * D 行列は、語彙数|V|が大きすぎる
    - 入力層の埋め込みベクトルを得る操作をtokenから射影fを使う操作に置き換える
    - この射影はタスクに合わせて学習可能となり、長距離依存を扱えるpowerfulなencodeを提供する一方でfoot printは小さい
- convolution & attention over projections

### 改善の検証
#### 文分類
- NLPの伝統的なタスクの一つ応用範囲は広い
    - spam detection
    - product categorization
    - sentiment classification
    - document retrieval
    - ranking

- 手法
    - sparse lexcial features like N-grams used by linear or kernels
    - CNN, LSTM, hi-attention mechanismがその性能を改善している

- on-deviseで動くことの重要性が高まっている
    - セキュリティ、プライバシー
    - 難しい点
        - 計算リソースが小さいこと
        - パフォーマンスは落ちてはいけないこと
    - Short text classificationでは
        - self-govering general networks(SGNN)や、その改善版がRNNをout-perform

#### 評価のモチベ
    - hyper parmの変化によるacc vs model size

#### データセット
Civil_commentsデータ
#### 比較手法
BERT
#### 結果
AUC
BERT 0.976
PRADO 0.963

パラメタサイズは
BERT    1億1000万 * 32bit
PRADO   130万 * 8bit




## 先行研究と比べてどこがすごい?
## 技術や手法のポイントはどこ?
## どうやって有効だと検証した?
## 議論はある?
## 次に読むべき論文は?

## 関連文献
    [](https://ai.googleblog.com/2020/09/advancing-nlp-with-efficient-projection.html?m=1&s=09)