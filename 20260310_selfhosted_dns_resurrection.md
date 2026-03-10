# 間違えて吹っ飛ばしたDNSを復旧するかどうかを考える

## 経緯
- minifluxでrssを管理している
- minifluxは同一VMのローカルで配信しているrssを参照している
- サーバーのIPはDHCPで固定していないため、変動する可能性がある
- minifluxはネットワーク上のxmlを参照し、フィードを獲得できる
- ネットワーク上のfeed.xmlの位置 URI が変化すると、フィードの連続性が失われる
- フィードの連続性が失われることを阻止するために、生のIPではなくDNSを設定した

しかし、使い方を忘れていたのでDNSを吹っ飛ばしてしまった。

## 現在の DNS クライアントの設定

```
Global
         Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
  resolv.conf mode: stub

Link 2 (ens18)
    Current Scopes: DNS
         Protocols: +DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 203.210.88.130
       DNS Servers: 192.168.1.xxx 203.210.88.130

Link 3 (tailscale0)
    Current Scopes: DNS
         Protocols: -DefaultRoute -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 100.100.100.100
       DNS Servers: 100.100.100.100
```

Global には stubが設定されている。（stubはマシン内の名前解決リクエストを一時的に肩代わりし、その内容をキャッシュする）

Link2 にはDHCPが払い出している`192.168.1.xxx`とnet3が管理する`203.210.88.130`が設定されている。

Link3 には tailscale の DNSが設定されている。

## DNS いるのか問題
現状の運用を考えると、必要ないことが分かった。
変更点を記載する。

## 変更点

### 以前
- local dns を構築した
- 仮想マシンレベルでそのdnsに名前解決の一部を担わせていた
- local dnsを使っているマシンは1VMのみだった

### 今回
- local dns を廃止した
- 名前解決したい影響範囲が、仮想マシンレベルでなく、あるコンテナのみだったのでそれに合わせて設定を記載した
- コンテナレベルで名前解決をしたい場合は、docker-compose.yml に `extra_hosts`ディレクティブを追記し、ドメインネームとipの対応を記載する
  - この操作は、コンテナ内の`etc/hosts`にエントリーを追記するよう振る舞う

今回はこれで解決した。