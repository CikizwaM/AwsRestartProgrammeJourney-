# 💰 S3 Static Hosting — Cost Analysis

S3 static hosting is pay-as-you-go with no fixed monthly fee. Cost is driven by three things:

| Cost driver | What it means | Typical rate | In Rands (ZAR) |
|---|---|---|---|
| Storage | GB of data sitting in the bucket, per month | ~$0.023/GB/month | R0.38 |
| Requests | Each GET/PUT/LIST call | ~$0.0004 per 1,000 GET requests | R0.00065 |
| Data transfer out | Data sent to visitors' browsers | ~$0.09/GB | R1.47 |

Of the three, data transfer out is almost always the dominant cost for a live site so storage and requests stay cheap even at scale, but as each visitor downloads the page's files, data transfer out can increase rapidly.

![Billing and Cost Management dashboard](./Images/S3/5.%20Billing%20and%20Cost%20Management%20Dashboard.png)

## 📈 Cost scaling with traffic

A page load pulls down the HTML, CSS, JS, and images — roughly 15MB total for this site as currently built.

| Monthly visitors | Approx. data transferred | Approx. monthly cost | In Rands (ZAR) |
|---|---|---|---|
| 1,000 | 15 GB | $1.40 | R23.00 |
| 10,000 | 150 GB | $13.50 | R220.00 |
| 100,000 | 1.5 TB | $135.00 | R2,204.00 |
| 1,000,000 | 15 TB | $1,350.00 | R22,041.00 |

Cost rises roughly linearly with traffic. A sudden traffic spike (e.g. a promotion or viral post) directly increases the bill in the same billing cycle, and there's no cap unless one is explicitly configured.

Image compression can decrease the transfer bill by 10x or more for the same traffic so the single biggest lever for controlling costs on an image-heavy site like a gym.

## 💡 Bottom line for an actual deployment

For a small local gym's marketing site with modest traffic (roughly a few thousand visitors per month) and optimized images, the realistic cost is under R100/month on S3 alone. Traffic combined with image weight is the number that moves the most so a business expecting growth or planning promotions should budget for data transfer scaling accordingly.
