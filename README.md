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

## 関連

Google 側の設定の経緯と、なぜこのサイトが必要になったかは
`~/workspace/machine/dev/gcloud.md` に記録してある。
