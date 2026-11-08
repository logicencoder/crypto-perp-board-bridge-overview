# Crypto Perp Board — Perpetual Futures Markets

![Perpetual futures market overview](assets/deriv-board-funding-open-interest-volume-table.png)

Funding rates, open interest and trading volume each describe a different part of a derivatives market, and every venue publishes them in its own format. Crypto Perp Board collects the public data of perpetual futures on five venues (Binance, Bybit, Hyperliquid, Aster and Lighter) into one workspace, so traders and researchers can compare the cost of holding a position, see where open interest sits and spot crowded markets without opening five sites. It runs on [logicencoder.com](https://logicencoder.com) from public endpoints and needs no exchange account.

## Tech stack

| Layer | Technologies |
|---|---|
| Collection | Python worker polling the public APIs of Binance USD-M, Bybit linear, Hyperliquid, Aster and Lighter every two minutes, with no API keys |
| History | SQLite snapshots per venue, symbol and time |
| Delivery | Authenticated REST ingest into a WordPress plugin written in PHP, with page caching switched off for the live view |
| Interface | JavaScript workspace with sortable, filterable, paged tables, histograms and leaderboards |

## Overview

The Overview shows a snapshot of the whole board: the number of perpetual markets tracked across the five venues, combined 24-hour volume and combined open interest, the markets with the highest and lowest funding right now, how many markets have positive funding (longs pay shorts) and how many negative (shorts pay longs), and the split of markets by venue. Each venue publishes the top markets by volume, so the board always covers the liquid part of the market.

## Markets

The Markets view is the full table: one row per venue and symbol with mark price, funding rate, time to the next funding, open interest, its change against 24 hours ago, 24-hour volume and a small funding trend drawn from stored history. Venue filter buttons narrow the table to one venue, any column sorts, and the table is paged. Funding rates with an absolute value of 0.05% or more are highlighted as the crowded side.

## Funding

The Funding view studies the rate itself: a histogram of funding across markets, the extremes at both ends and a cross-venue funding difference for the same symbol, which shows where one venue charges noticeably more or less than another. Rates are shown per eight hours with the countdown to the next settlement, and an annualised figure (APR) assumes three settlements a day. Binance, Bybit and Aster publish a settled rate; Hyperliquid settles hourly and is scaled to eight hours; Lighter publishes no funding rate, so its figure is an estimate from the gap between mark and index price and is marked as such. Read it as an observation of what longs or shorts pay, not as a forecast of price.

## Open Interest and Volume

Open Interest is a leaderboard of where the money sits, ranked by the value of open positions and showing the change over 24 hours. Volume ranks the 24-hour turnover and adds the ratio of open interest to volume, which separates markets that are mostly traded from markets where positions are mostly held. Switching between the two views for the same symbol is the quickest way to tell a busy market from a heavily positioned one.

## Carry

Carry is the board's own derived metric: the funding that a basis position (long spot against a short perpetual) would have collected or paid over the last seven days, together with the number of hours the history covers. Every stored funding rate is weighted by the time it was in force, so many individual funding payments become one comparable figure per market, ranked from most positive (shorts collected) to most negative. It excludes trading fees, spreads and margin costs, and for markets that settle more often than every eight hours it is an approximation.

## Crowding

Crowding is a second derived score for squeeze risk. For every market that has both a funding rate and open interest it multiplies the market's rank by absolute funding with its rank by open interest (each as a percentile among the markets shown) and scales the product to 0 to 100, so a market where one side pays a lot and positions are large ranks highest. Markets without open-interest data, such as Binance and Aster symbols outside the top 25 by volume, are not scored. It is a screening score defined by this board, not a probability.

## Guide and settings

The Guide defines funding, open interest, APR and the cross-venue difference. Settings adjust the accent colour, font, density and rows per page. The dashboard refreshes on its own every few seconds from the latest snapshot, and all figures carry the time of the last snapshot in UTC.

## Scope and limits

The board uses public data from the five venues and shows the 40 markets with the highest 24-hour volume of each. Open interest is fetched per symbol for the 25 largest markets on Binance and Aster and for all rows on the other venues. Funding figures describe what has been charged, derived metrics are the board's own definitions, and none of it is investment advice. Leveraged derivatives carry a high risk of loss, and the offering of such products to retail clients is regulated in many countries.

---

Private code: [crypto-perp-board-bridge](https://github.com/logicencoder/crypto-perp-board-bridge)

See [REPOS.md](REPOS.md) for related repositories.

**[Logic Encoder](https://logicencoder.com)** · [GitHub](https://github.com/logicencoder) · [Contact](https://logicencoder.com/contact/)
