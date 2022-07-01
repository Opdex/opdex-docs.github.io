---
layout: page
title: Providing Liquidity
parent: Knowledge Center
nav_order: 4
---

Providing liquidity is when a user lends tokens to a liquidity pool's reserves and in return collects transaction fees from swap transactions. Transaction fees, remain in the liquidity pool and add to the token reserves rather than being split between providers immediately.

When providing liquidity, OLPT are returned based on the ratio of liquidity provided compared to the total liquidity in the pool. OLPT are SRC-20 tokens just like others but they represent a percentage of liquidity in a pool. For example, a user adds and is responsible for 10% of the total liquidity in a pool, then they will be minted and receive 10% of the total OLPT supply.

Held OLPT can be burned at any time for the percentage of the pool's reserve tokens. When a user wants to remove their liquidity from a pool, they'll burn OLPT and be returned the pool's underlying CRS and SRC tokens.

<iframe width="100%" style="aspect-ratio: 16/9;" src="https://www.youtube-nocookie.com/embed/BahWVovdN_c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Adding Liquidity

Adding liquidity requires adding both CRS and SRC tokens to a liquidity pool, and the ratio between the two must match the current liquidity pool reserves ratios.

For example, a pool that has 1 CRS token for every 10 SRC tokens would require the user to match that ratio. A user looking to provide 10 CRS tokens would also need to provide 100 SRC tokens, matching the ratio.

Upon providing liquidity, user's will receive OLPT to hold. The percentage of the total supply of OLPT held by the user represents the percentage of the pool's liquidity the user owns.

While providing liquidity, token swaps will incur transaction fees and those fees will remain in the pool's reserves. At any time, user's can remove their liquidity by burning OLPT and receiving their percentage of the pool's CRS and SRC token reserves.

{: .warning }
When providing liquidity, there are risks such as impermanent loss. In short, if user's provide and reserve ratios change by the time liquidity is removed, it may have been better to hold the tokens rather than ever providing. Prior to participating in liquidity provisioning, it is critical to do your own research.

## Removing Liquidity

Liquidity providers can remove their liquidity at any time by burning held OLPT. The percentage of the OLPT supply burned, is the percentage of liquidity pool reserve tokens returned to the user, including transaction fees.
