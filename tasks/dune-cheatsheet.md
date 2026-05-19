# Dune SQL 速查卡（BD 专用精简版）

## 常用表

| 你要查 | 表名 |
|--------|------|
| 代币价格 | `prices.usd` |
| ERC20 转账记录 | `erc20_ethereum.evt_Transfer` |
| ETH 转账 | `ethereum.traces` |
| DEX 交易 | `dex.trades` |
| NFT 交易 | `nft.trades` |
| 标签地址 | `labels. wallets` |

## 常用字段

| 字段 | 说明 |
|------|------|
| `block_time` | 交易时间 |
| `tx_hash` | 交易哈希 |
| `from` / `to` | 发送方 / 接收方地址 |
| `value / amount` | 金额（注意单位，常为 wei） |
| `symbol` | 代币符号 |
| `contract_address` | 合约地址 |

## 必会语法

```sql
-- 查价格
SELECT *
FROM prices.usd
WHERE symbol = 'ETH'
  AND blockchain = 'ethereum'
ORDER BY minute DESC
LIMIT 1

-- 按天聚合
SELECT date_trunc('day', block_time) AS day,
       COUNT(*) AS tx_count
FROM dex.trades
WHERE block_time >= now() - interval '7' day
GROUP BY 1
ORDER BY 1

-- 金额换算（USDC 6位小数）
SELECT value / 1e6 AS amount_usdc
FROM erc20_ethereum.evt_Transfer
WHERE contract_address = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48

-- 多表 JOIN（挖巨鲸流向）
SELECT t.from, t.to, p.price
FROM dex.trades t
JOIN prices.usd p ON p.minute = date_trunc('hour', t.block_time)
  AND p.symbol = 'ETH'
```

## BD 场景 SQL 模板

### 找巨鲸（大额转账）
```sql
SELECT *
FROM erc20_ethereum.evt_Transfer
WHERE value >= 1000000 * 1e6  -- 100万 USDC
  AND block_time >= now() - interval '1' day
ORDER BY value DESC
```

### 查协议活跃度（7天交易量）
```sql
SELECT date_trunc('day', block_time) AS day,
       COUNT(DISTINCT taker) AS uaw,
       SUM(amount_usd) AS volume_usd
FROM dex.trades
WHERE project = 'uniswap'
  AND block_time >= now() - interval '7' day
GROUP BY 1
ORDER BY 1
```

### 持币者分布
```sql
SELECT
  CASE
    WHEN balance_usd < 1000 THEN '<1K'
    WHEN balance_usd < 10000 THEN '1K-10K'
    WHEN balance_usd < 100000 THEN '10K-100K'
    ELSE '100K+'
  END AS bucket,
  COUNT(*) AS holders
FROM token_balances
WHERE symbol = 'TOKEN'
GROUP BY 1
```

## 常见坑

- **金额单位**：USDC=1e6，ETH/WETH=1e18，忘了直接多好几个零
- **时间过滤**：`block_time >= now() - interval '7' day`，别手写日期
- **address 大小写**：Dune 里全小写，查的时候 `LOWER('0xABC...')`
- **表名带版本**：`dex.trades` 已废弃就试 `dex_ethereum.trades`

## 快捷键

| 操作 | 快捷键 |
|------|--------|
| 运行 | `Cmd/Ctrl + Enter` |
| 格式化 SQL | `Shift + Cmd/Ctrl + F` |
| 新建查询 | `Cmd/Ctrl + N` |
| Fork 别人看板 | 点右上角 Fork 按钮 |
