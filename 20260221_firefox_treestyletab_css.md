## Firefoxの tree style tab で上部の tab を非表示にする
- `about:config`で`toolkit.legacyUserProfileCustomizations.stylesheets` -> `true`に設定
- `about:profile`のルートディレクトリ内に`chrome`フォルダを作成する
- `chrome`フォルダ内に`userChrome.css`を作成する

以下が`userChrome.css`
```userChrome.css
#tabbrowser-tabs {
visibility: collapse !important;
min-height: 0 !important;
}

tab {
display:none !important;
}
```

## 参照
- https://github.com/piroor/treestyletab/wiki/Code-snippets-for-custom-style-rules#slightly-betterworse-option-for-hiding-tabs-depending-on-what-you-want
