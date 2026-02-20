## BAM とは
遺伝子発現データの中で、BAM や SAM と呼ばれているものは、リード（計測値）とリファレンス（辞書）をマッピングした後のデータである。BAMとSAMがもつ情報は本質的には同一で、バイナリ形式かテキスト形式かどうかで名称が変わる。

フォーマットは https://bi.biopapyrus.jp/format/sam.html にて概要がまとめられているので、その一部を流用する

```
@HD	VN:1.0	SO:unsorted
@SQ	SN:1	LN:248956422
@SQ	SN:10	LN:133797422
@SQ	SN:11	LN:135086622
@SQ	SN:12	LN:133275309

## 中略

ERR030890.64313195	0	9	35766194	39	65M	*	0		CTCATTGTGGTTTTAATTTGCATTTTCCTAATGTTTAATGACATTGAGCATCTTTTTATGTGCTT	FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF<<<<FFF=F	AS:i:0	XS:i:-38	XN:i:0	XM:i:0	XO:i:0	XG:i:0	NM:i:0	MD:Z:65	YT:Z:UU
ERR030890.64313017	0	6	75917822	42	65M	*	0		AGACATTTTCATTCACAGGTCATTACTATGGTTCTCAGCGATCCAAATATGTAGATCATTGGTTT	FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF<FFF<FFFFFFFF<F	AS:i:0	XN:i:0	XM:i:0	XO:i:0	XG:i:0	NM:i:0	MD:Z:65	YT:Z:UU
```

何が分かるかというと、データのヘッダー（@HD）、（おそらく）シーケンス情報（@SQ）**(まとめてタグと呼ばれる)**。そして、実データ部（アラインメント）である。

ヘッダーにはアラインメントの情報（ソート済みかどうかや、SAMフォーマットのversion）、リファレンス配列の情報、データのマッピングに用いられたコマンドやそのオプションが記載される。

アラインメントにはデータがタブ区切りで並んでいる。
最小要件では11カラムの情報が必要で、ツールによっては12カラム以降に独自の情報を付け足す拡張を行っている。

## SAM・BAM間のフォーマット変換
- samtools で行う

## 参考
- https://www.ddbj.nig.ac.jp/dra/datafile.html#bam