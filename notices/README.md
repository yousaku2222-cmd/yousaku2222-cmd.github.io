# 運営からのお知らせ

各アプリは起動時に `https://yousaku2222-cmd.github.io/notices/<アプリID>.json` を読み込み、
未読のお知らせを起動時ダイアログ（1回だけ）と「お知らせ」一覧に表示する。
ファイルを更新して push すれば反映される（アプリの再提出は不要。GitHub Pages のキャッシュで最大10分ほど遅れる）。

| アプリ | ファイル | 表示言語 |
|---|---|---|
| Anniv | `anniv.json` | 日本語固定 |
| トーク保存 | `talk_saver.json` | 端末の言語（ja/en/ko/zh/zh_Hant/id/th） |
| 16タイプ診断 | `mbti.json` | 日本語固定 |
| Yabai Word | `yabai_word.json` | 英語固定（`en` が無ければ `ja`） |
| 軍帥儀 | `gunsuigi.json` | 日本語固定 |
| 術牌 | `jutsufuda.json` | 日本語固定 |

## 書式

```json
{
  "notices": [
    {
      "id": "2026-09-28-ad-pause",
      "date": "2026-09-28",
      "title": { "ja": "広告の一時停止について", "en": "About ads" },
      "body": { "ja": "本文。改行は \n で。", "en": "Body text." },
      "start": "2026-09-28T00:00:00+09:00",
      "end": "2026-10-27T00:00:00+09:00",
      "platforms": ["ios", "android"],
      "minVersion": "1.0.0",
      "maxVersion": "1.2.9",
      "url": "https://apps.apple.com/...",
      "urlLabel": { "ja": "アップデートする" },
      "popup": true
    }
  ]
}
```

| 項目 | 必須 | 説明 |
|---|---|---|
| `id` | ○ | 一意な文字列。既読管理に使うので**一度出したら変えない**。変えると全員に未読として再表示される |
| `date` | ○ | 一覧に出す日付。新しい順に並ぶ |
| `title` | ○ | 文字列（日本語扱い）か、言語コード→文字列 |
| `body` | | 同上 |
| `start` / `end` | | この期間だけ表示（`end` ちょうどで消える）。省略で無期限 |
| `platforms` | | `ios` / `android`。省略で両方 |
| `minVersion` / `maxVersion` | | アプリのバージョンがこの範囲の人だけ（両端を含む）。「古い版の人にアップデートを促す」なら `maxVersion` |
| `url` / `urlLabel` | | ダイアログにリンクボタンを出す。ラベル省略時は「詳しく見る」 |
| `popup` | | `false` で起動時ダイアログを出さず一覧だけに載せる。省略時 `true` |

言語は 端末の言語（`zh_Hant` → `zh`）→ `ja` → `en` の順に探す。
JSON が壊れていると**そのアプリのお知らせが全部出なくなる**ので、push 前に `node -e "JSON.parse(require('fs').readFileSync('notices/xxx.json','utf8'))"` などで確認する。
