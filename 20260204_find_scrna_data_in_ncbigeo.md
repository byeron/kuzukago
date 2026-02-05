# はじめに
[NCBI GEO](https://www.ncbi.nlm.nih.gov/geo/) はオミクスデータのリポジトリである。
生命系の論文の Data availability を参照すると、多くのケースで NCBI GEO リポジトリにデータセットが格納されている。
特に scRNA-seq データは実験コストが大きいため、既存のデータベースを再活用することが重要である。
一方、NCBI GEO のデータ管理に用いられる用語や操作は初見だと把握しにくいため、ある程度の慣れが必要となる。

# 目的
- 論文中に参照されている `scRNA-seq データ` を探して、ダウンロードできるようにすること

# 論文の Data availability
近年の論文では、Data availability などの名前で、実験で計測されたデータの参照先を明記することが求められる。
例えば、[このオミクスデータ解析の論文](https://doi.org/10.1038/s41467-024-53849-3)の Data availability の項目には、
プロテオミクスのデータや bulkRNA-seq のデータ、scRNA-seq のデータが掲載されている。

<img width="785" height="487" alt="image" src="https://github.com/user-attachments/assets/031d3708-54d5-46ad-8b80-576cc56bf2f6" />

# NCBI GEO リポジトリの使い方
- GSE: SEries、研究の単位でそのIDのことを accession No. とも呼ぶ
  - *Samples* に研究で取り扱われた検体（GSMxxxxxxx）のリストが格納されている
  - *Experiment type* にデータの種別が記載されている
    - 例えば、`Expression profiling by high throughput sequencing`とあれば次世代シーケンサーのデータである
    - *Summary* に scRNA-seq やマイクロアレイなどの種別が書かれることがある
  - GPL: PLatform、計測装置や型番などが書いてある（見なくてもいい）
- GSM: SaMple、1検体単位のデータが格納されている
  - GSM の各ページには、Organism や 年齢、臓器などのメタ情報が格納されている
  - `Supplementary file`: **ここにデータ本体のリンクがある**

要するに確認する順序は以下のとおりである。
1. 論文に示された`GSE`を参照する
2. **GSE ページ内** >  *Experiment type* や *Summary* からデータ形式を確認する
3. GSE ページ内 > *Samples* より各検体のデータにアクセスする
4. **GSM ページ内** > 検体のメタ情報を確認する
5. GSM ページ内 > *Supplementary file*よりカウントデータなどを実データをダウンロードする

## GSE ページ（データ全体の概要を示す）の例
優先的に確認する点を赤字で示す。

<img width="694" height="964" alt="image" src="https://github.com/user-attachments/assets/abe9b39b-51c4-42c0-b9f1-be4c78125c78" />


## GSM ページ（各検体の情報や、実データが格納）の例
<img width="696" height="1011" alt="image" src="https://github.com/user-attachments/assets/49d7249c-1c8a-4ecd-bbb2-f0c8c15dcbed" />

## NCBI GEO の検索システム
[NCBI GEOの検索](https://www.ncbi.nlm.nih.gov/geo/browse/)

`MGUS`などの疾病の名称や`scRNA-seq`などのフリーワードで検索できる。
ただし、列名を見る限り `scRNA-seq`という分類で登録されているのではなく、データのタイトルに含まれる`[scRNA-seq]`という文字列を拾っている。

分類名だけに着目すると *Series type* に `Expression profiling by high throuput sequencing` とあり、これが次世代シーケンサーで獲得されたデータであることを示している。
ただし、単一細胞かバルクかは各シリーズのsummaryを参照しないとわからない。

<img width="2312" height="549" alt="image" src="https://github.com/user-attachments/assets/52cbb4e0-2726-43a6-b7ef-5373af8e5c2a" />

### 検索システムの便利な使い方
適当に疾病名を入力すると関連するデータセットのリストを取得できる。各シリーズのページから論文も参照できるので、情報収集にもよい。
また、サンプル数やデータセットの提供形式なども一覧で確認できるので、場合によっては論文からデータを探すよりもラク。
