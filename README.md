# igmap

Instagramの投稿をGoogleマップ上の地点に紐付けて公開するWebマップ。

- 公開地図: https://wakayamayamorisha.github.io/igmap/
- 投稿管理画面: https://wakayamayamorisha.github.io/igmap/admin/ （GASウェブアプリへ転送）

## このリポジトリの中身

| ファイル | 役割 |
|---|---|
| `index.html` | 公開地図。`spots.json` を読み込んでマーカーを立てる |
| `spots.json` | 地点データ。投稿管理画面（GAS）が書き換える |
| `admin/index.html` | 投稿管理画面（GAS）への転送ページ |
| `images/` | Instagramに投稿する写真の置き場。GASがコミットする |

投稿管理画面のソースは別フォルダ（`AI_agents/yamori-agents/igmap/gas`）で clasp 管理。

## spots.json の形式

```json
[
  {
    "id": "ramen-maruta",
    "name": "〇〇ラーメン",
    "lat": 34.2301,
    "lng": 135.1702,
    "note": "",
    "ig": ["https://www.instagram.com/p/AbCdEf123/"]
  }
]
```

- `id` … 共有URLの `#id` に使われる。管理画面が自動生成する（英数字がなければ `spot-1` 形式）
- `ig` … 同じ地点の投稿を並べる。新しい投稿は末尾に追加される

## 仕組み

1. 投稿管理画面（GAS）で店名・位置・写真・本文を入力
2. GASが写真を `images/` にコミット → `raw.githubusercontent.com` の公開URLができる
3. Instagram コンテンツ公開APIで投稿 → permalink を取得
4. GASが `spots.json` を更新してコミット
5. GitHub Pagesが再デプロイ（1〜2分）→ 地図に反映

「すでに投稿済みのInstagram URLを貼る」モードはInstagram APIを使わないので、トークンなしで今すぐ使える。

## 注意

- `index.html` のMaps APIキーは公開前提（リファラー制限＋API制限で保護）。それ以外の秘密情報はこのリポジトリに入れない
- GitHubトークン／Instagramトークンはすべてスクリプトプロパティに保管する
