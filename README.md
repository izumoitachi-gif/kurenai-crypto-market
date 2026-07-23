# kurenai-crypto-market (Layer4: crypto専用リポ)

`izumoitachi-gif/discord-rss-notifier` の GitHub Actions同時実行20並列プール(Free plan)から
crypto系ワークロード(価格poll + 21RSS 4並列 flash + debug監視)を切り出した独立リポ。

## 収録workflow
- `crypto_price_notify` — BTC/ETH/SOL 5分毎価格poll+閾値超過通知+速報検知
- `crypto_flash_A/B/C/D` — 21crypto RSS を4並列(5〜6RSS/フック)で走査・BREAKING_TERMSで速報検知
- `crypto_debug_late` — scheduled発火の詰まり検知(発火成功時はログ書かず)

## 発火経路(Layer3+Layer4)
1. `izumoitachi-gif/kurenai-market-trigger` の trigger.yml (cron 1,16,31,46)
2. → `discord-rss-notifier` (market_notify) と このリポ (crypto 5workflow) を dispatch API で叩く
3. → 各リポの GitHub Actions concurrent 20枠を独立占有 → 21RSS並列でも詰まらない

## 参照
- 設計: `.claude/中期記憶/Discord/金融市場Bot_パットンMS_20260716/08_発火経路多重化_パパ設計.md`
- 要件: `.claude/中期記憶/Discord/金融市場Bot_パットンMS_20260716/07_要件表.md`
