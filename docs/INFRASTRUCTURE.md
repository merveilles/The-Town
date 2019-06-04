# Infrastructure

In an effort to be transparent and in the spirit of collaboration, this document outlines the infrastructure and recurring cost of the [Town of Merveilles](https://merveilles.town).

## Server
Merveilles.town is an instance of **[Mastodon](https://joinmastodon.org) version 2.8.0** that is on a virtual private server (VPS) hosted on **[DigitalOcean](http://digitalocean.com/)**. The VPS is hosted in **DigitalOcean**'s `FRA1` region, located in Frankfurt, Germany. It is a manual install, done by going through the [installation instructions](https://docs.joinmastodon.org/administration/installation/) outlined in the [documentation](https://docs.joinmastodon.org/). The VPS operating system is **64-bit [Ubuntu](https://www.ubuntu.com/) version 18.10**, created using **DigitalOcean**'s one-click install. To see the tools powering **Mastodon**, you can reference its [GitHub Page](https://github.com/tootsuite/mastodon).

## Assets
All images, videos, and other files you can upload to the instance are hosted on **[Amazon S3](https://aws.amazon.com/s3/)**. The bucket (the location containing all assets) is hosted in the `eu-central-1` region, also located in Frankfurt, Germany.

## Emails
Email notifications sent from `notifications@email.merveilles.town`, used for password resets and the like, are sent through **[Sparkpost](https://www.sparkpost.com/)**.
We also have an account and receive emails at [Fastmail](https://fastmail.com/) for our various services.

## Domain Registration
The [merveilles.town](https://merveilles.town) domain is registered through **[Dreamhost](https://dreamhost.com/)** and is set to automatically renew before the registration expires.

# Cost
| Resource | Cost | Reference | Notes |
|----------|------|-----------|-------|
| DigitalOcean | **USD$24.00** per month | [https://www.digitalocean.com/pricing/](https://www.digitalocean.com/pricing/) | Droplets can be cheap, but Mastodon specifically needs at least 4Gb of memory to compile assets. I also have weekly backups enabled, which costs an extra 20% of the droplet cost. |
| Dreamhost Registration | **USD$2.92** per month | [https://www.dreamhost.com/legal/domain-registration-terms/](https://www.dreamhost.com/legal/domain-registration-terms/) | The `.town` top-level domain (TLD) is a bit more expensive to register than other TLDs, which costs USD$34.99 per year.. |
| Amazon S3 | About **USD$11.00** per month | [https://aws.amazon.com/free/](https://aws.amazon.com/free/) | The instance is currently on the free tier of S3, but you only pay as much as you use, which is based on the number of variables. The current amount is based on the cost from the last few months of use. |
| Fastmail | **USD$4.17** per month | [https://www.fastmail.com/pricing/](https://www.fastmail.com/pricing/) | We need to have the Standard tier so that we can use our own domain. We pay for the yearly license, $50/year, which works out to be |
| Sparkpost | **USD$0.00** per month | [https://www.sparkpost.com/pricing/](https://www.sparkpost.com/pricing/) |  The free tier of SparkPost allows for 15000 emails sent per month, which we should be able to comfortably stay below with a smaller number of users. |

## Total Cost
At least **USD$66.09 per month**. This cost can be higher based on the amount of resources the instance uses.
