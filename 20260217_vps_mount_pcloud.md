## はじめに
昨今における日本の夏は非常に高温である。
また、PCパーツの高騰もすさまじく、一度パーツが破損すると数万単位の出費につながる。
そこで、自宅サーバーでよく使う機能のみを運用する`避暑地VPS`の必要性を考えた。

今回は、ざっくりと避暑地VPSの構成要素とざっくりとした構築手順をまとめる

## VPS
- https://greencloudvps.com/billing/store/budget-kvm-sale
- https://pasokon.blog/articles/2025-02-12-openclaw-greencloud/
- https://portablecode.info/2025/12/27/vps-greencloud-review/

年額25ドルの *green cloud* のbadget plan が一部の人で人気である。
たしかに、国内の業者の*kagoya*や*xserver*でもドル換算だと月5ドルくらい、*green cloud* は`2ドル`で非常に安い。

レビューを見ると、定期的に在庫が補充されているようなのでタイミングを見計らわないといけないが、見かけたら即決で契約していいかもしれない。

## VPS のセットアップ
- https://portablecode.info/2025/12/27/vps-greencloud-review/

再掲だが、やりたいことはほとんどこの人の記事で記載されている。具体的には以下である。
- 初期セットアップ
- ファイアウォール設定
- tailscale の導入

## ストレージ（pCloud）
VPSのデータを永続化するために pCloud のディレクトリをマウントする。
pCloud 公式の手法ではGUIによる操作が必要であるため、ヘッドレスサーバーでは不適である。
そこで、RCloneを用いて pCloud をマウントする方法をまとめる。

- https://rclone.org/pcloud/
- https://rclone.org/remote_setup/
- （参考）https://superuser.com/questions/1315199/change-default-mount-point-of-pcloud

rclone の公式サイトで手順が記載されている手法をそのまま試せばよさそう。
ざっくりとした手続きを確認すると、

```
rclone config  # 対話型で設定を開始
```

```
No remotes found, make a new one?  # 新規プロファイル作成
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
```

```
name> remote  # プロファイルの名前を設定
```

```
Type of storage to configure.  # 接続先のストレージを選択（選択肢の中から選ぶ、数値でも可）
Choose a number from below, or type in your own value
[snip]
XX / Pcloud
   \ "pcloud"
[snip]
Storage> pcloud
```
```
# 何かしらのクレデンシャルを設定（ブランク可）
Pcloud App Client Id - leave blank normally.
client_id> 
Pcloud App Client Secret - leave blank normally.
client_secret> 
```
```
Edit advanced config?
y) Yes
n) No (default)
y/n> n

# ここで ヘッドレスサーバーの場合は n を選ぶらしい
Remote config
Use web browser to automatically authenticate rclone with remote?
 * Say Y if the machine running rclone has a web browser you can use
 * Say N if running rclone on a (remote) machine without web browser access
If not sure try Y. If Y failed, try N.

y) Yes (default)
n) No
y/n> n

# 別の端末で認証作業を行う
Option config_token.
For this to work, you will need rclone available on a machine that has
a web browser available.
For more help and alternate methods see: https://rclone.org/remote_setup/
Execute the following on the machine with the web browser (same rclone
version recommended):
        rclone authorize "onedrive"
Then paste the result.
Enter a value.
config_token>
```

別マシンで`rclone authorize ${storage name}`を実行し、コードを取得する
```
rclone authorize "onedrive"
NOTICE: Make sure your Redirect URL is set to "http://localhost:53682/" in your custom config.
NOTICE: If your browser doesn't open automatically go to the following link: http://127.0.0.1:53682/auth?state=xxxxxxxxxxxxxxxxxxxxxx
NOTICE: Log in and authorize rclone for access
NOTICE: Waiting for code...

Got code
Paste the following into your remote machine --->
SECRET_TOKEN
<---End paste
```

ヘッドレスサーバーに戻り `config_token>`にトークンをペーストする

#### 補足
設定ファイルを場所を取得できる。取得したファイルは別のリモートマシンで使うことができる。つまり、作業自体はローカルサーバで行い、出来上がった設定ファイルを別のサーバーに持って行ってよい。
```
rclone config file
Configuration file is stored at:
/home/user/.rclone.conf
```

## セルフホストするソフトウェア
- linkding

必須はこれくらいかもしれない。data フォルダーと設定ファイルを pcloud に配置し、そのディレクトリ2つをマウントして運用する。VPS上は docker engine などの消えても困らないものだけ載せる

## 電気代

> **bru ラズパイの消費電力が10W、電気代にすると月2USDくらいだから、VPSはお買い得だよね。

らしい

