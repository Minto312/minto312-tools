# minto312-tools

個人用ツール (`karinto tools`) のホームページとプライバシーポリシー。

Google Cloud プロジェクト `karinto-personal` の OAuth 同意画面が
**Application home page** と **Privacy policy** の URL を要求するために用意した。
中身を読ませることが目的ではなく、同意画面を In production に上げるための要件を満たすのが目的。

## 構成

| パス | 役割 |
| --- | --- |
| `index.html` | ホームページ (同意画面の Application home page) |
| `privacy/index.html` | プライバシーポリシー (同意画面の Privacy policy) |
| `CNAME` | `tools.minto312.com` |

静的 HTML のみ。ビルドもフレームワークも無い。

## 公開

GitHub Pages (`main` ブランチのルート) → `https://tools.minto312.com/`

DNS は Cloudflare で `tools` の CNAME を `minto312.github.io` へ。
**grey cloud (DNS only)** にすること (`portfolio.minto312.com` と同じ)。

### 🔴 TLS 証明書の発行が詰まったときは、独自ドメインを外して付け直す

構築時 (2026-09-16) に踏んだ。DNS は正しく引けて HTTP は 200 を返すのに、
**1 時間以上 `https_certificate` が `null` のまま**で、HTTPS では GitHub 既定の
`*.github.io` 証明書が出続けた。

```
curl: (60) SSL: no alternative certificate subject name matches target hostname
subject=CN=*.github.io
```

同じ値で `PUT .../pages -f cname=...` を投げ直しても動かない。
**一度空にしてから付け直すと即座に発行された** (`issued` → 約 15 秒で `approved`)。

```bash
gh api -X PUT repos/Minto312/minto312-tools/pages -f cname=""
gh api -X PUT repos/Minto312/minto312-tools/pages -f cname=tools.minto312.com
gh api repos/Minto312/minto312-tools/pages --jq '{cname,cert:.https_certificate.state}'
# 通ったら HTTPS 強制
gh api -X PUT repos/Minto312/minto312-tools/pages -F https_enforced=true
```

切り分けで潰したもの (どれも原因ではなかった): CAA レコード (そもそも無い)、
Cloudflare の proxy (grey cloud)、DNS 未伝播、ビルド失敗 (`built` / `error: null`)。
**陽性対照は `portfolio.minto312.com`** — 同じゾーン・同じ CNAME 先で HTTPS が通るので、
ドメインと Cloudflare の設定自体は無罪だと分かる。

⚠ 切り分け中、`curl: (6) Could not resolve host` で 40 回連続の空振りを
「証明書待ち」と誤読した。**develop のシステムリゾルバが一時的に引けなくなっていただけ**で、
`dig @1.1.1.1` は通っていた。**症状が変わったら生エラーを取り直すこと。**

## 関連

Google 側の設定の経緯と、なぜこのサイトが必要になったかは
`~/workspace/machine/dev/gcloud.md` に記録してある。
