# crypto-daily-report

独立的加密货币日报生成与发送项目。每天定时抓取价格与 PANEWS 内容，组装为固定格式日报，并通过 Cloudflare Worker relay 发送单张 Lark interactive card。

## 架构

```text
Cloudflare Cron (10:30 BJT)
  -> black-swan-mcp scheduled()
    -> GitHub workflow_dispatch
      -> scripts/crypto-daily-report.mjs
        -> CoinMarketCap / CryptoSlate
        -> PANEWS RSS + 页面
        -> Cloudflare Worker /send-lark
        -> Lark webhook
```

## 运行规则

- 调度时间：北京时间每天 `10:30`
- 调度源：Cloudflare Cron
- 执行方式：GitHub Actions `workflow_dispatch`
- 发送方式：只发送一条消息、只发送一张卡片
- 转发方式：必须经由 Cloudflare relay，不直接 POST Lark webhook
- 正文格式：遵循旧 Claude trigger 规则

## GitHub Secrets

- `CRYPTO_DAILY_RELAY_URL`
- `CRYPTO_DAILY_RELAY_SECRET`
- `CRYPTO_DAILY_LARK_WEBHOOK_URL`
- `CRYPTO_DAILY_LARK_WEBHOOK_SECRET`
- `CRYPTO_DAILY_DRY_RUN`

## 本地调试

```bash
node scripts/crypto-daily-report.mjs
```
