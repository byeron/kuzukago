## FASTQ データの探し方
研究関係者から提供される場合を除き、基本的にFASTQデータは公開リポジトリから取得する。
主な公開リポジトリは以下の2つである。
- NCBI GEO
  - https://www.ncbi.nlm.nih.gov/geo/browse/?view=series&sort=type
- SRA
  - https://www.ddbj.nig.ac.jp/dra/index.html
  - Sequence Read Archive の略
  - `SRA`形式で保存されており、取り扱うためには`SRA -> FASTQ`への変換が必要
  - SRAはバイナリ形式で、平文であるFASTQと比較して小さいデータ量で同一の情報を保持できる
  - SRA <--> FASTQ は相互変換可能

論文などに NCBI GEO、または、SRAのIDが記載されているため、それらを参照して各リポジトリのデータにアクセスする。

## こちらも参照
- https://www.ddbj.nig.ac.jp/dra/datafile.html