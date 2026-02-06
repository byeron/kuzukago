## はじめに
*scanpy* は scRNA-seq データを取り扱うためのツールである。
scRNA-seq データの処理は統計解析用プログラミング言語である R による取り扱いがメジャーであるが、近年は Python 用のライブラリ群である [scverse](https://scverse.org/packages/) による解析も多く報告されている。

## scverse
3つのデータ構造（*anndata*, *mudata*, *spatialdata*）とそれらを取り扱う解析用フレームワーク群の総称である。
発現変動遺伝子解析や次元圧縮、クラスタリングなどの基本的な発現量解析から、マルチモーダルデータ解析、空間解析などのフレームワークが提供されている。
基本的な解析は *anndata* と *scanpy* の組み合わせにより実施される。

## anndata
*anndata* とは、行にサンプル（細胞）、列に遺伝子の情報を持つ配列をコアにしたテーブルデータである。
その特徴は行方向や列方向に統計処理や次元圧縮、クラスタリングなどを適用し、行方向、または、列方向に追加情報を append できる点である。また、スパースデータにも対応している。

行は`obs`、列は`var`と呼ばれており、コアのテーブルは便宜上 `X` と記載されている。つまり、Pandasで言うところの `index`と`columns`、`values`が対応していると思えばいい。

例えば、細胞IDに対して、細胞Typeの情報を付加したいときは以下のようにする
```py
# ランダムでカテゴリデータを n_obs 分生成
ct = np.random.choice(["B", "T", "Monocyte"], size=(adata.n_obs,))

# obs（行）方向に cell_type という名称の新しい Series を追加
adata.obs["cell_type"] = pd.Categorical(ct) 
```

`obsp`や`varp`の`p`つきデータは、Seriesではなく*matrix*が格納される。
具体的には、PCAの主成分得点はサンプル（行）に対してPC1, PC2, PC3...のような列が必要である。
このような場合に`anndata.obsp["PCA"]`のような形式でアクセスし、データを格納する。
チュートリアルでは、UMAP の例が掲載されている。

```py
adata.obsm["X_umap"] = np.random.normal(0, 1, size=(adata.n_obs, 2))
adata.varm["gene_stuff"] = np.random.normal(0, 1, size=(adata.n_vars, 5))
```

登録したデータは、`print(anndata)`などで概要を確認できる。
具体的には以下のような情報が出力される。
```py
AnnData object with n_obs × n_vars = 100 × 2000
    obs: 'cell_type'
    obsm: 'X_umap'
    varm: 'gene_stuff'
```

`uns`はおそらく*unstructured*、非構造データを示す。
obsやvarに直接対応しないメタ情報などの格納に用いる。

`layer`は、正規化後のデータや対数変換後のデータなど、元のデータ`X`と同じ次元を持つデータを格納する概念である。

最後に、anndataのコンストラクタは以下のように呼び出す。
```py
counts = csr_matrix(np.random.poisson(1, size=(100, 2000)), dtype=np.float32)
adata = ad.AnnData(counts)
```
つまり、`X`に相当するデータ（カウント行列、TPMなど）のnumpy行列を渡せば最低限の動作は可能となる。

## scanpy
あとでかく
