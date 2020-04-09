# Infrastructure

In an effort to be transparent and in the spirit of collaboration, this document outlines the infrastructure and recurring cost of the [Town of Merveilles](https://merveilles.town).

## Server
Merveilles.town is an instance of **[Mastodon](https://joinmastodon.org) version 3.1.3** that is on a virtual private server (VPS), or "droplet", hosted on **[DigitalOcean](http://digitalocean.com/)**. The VPS is hosted in **DigitalOcean**'s `FRA1` region, located in Frankfurt, Germany. It is a manual install, done by going through the [installation instructions](https://docs.joinmastodon.org/admin/install/) outlined in the [documentation](https://docs.joinmastodon.org/). The VPS operating system is **64-bit [Ubuntu](https://www.ubuntu.com/) version 18.04 LTS**, created using **DigitalOcean**'s one-click install. To see the tools powering **Mastodon**, you can reference its [GitHub Page](https://github.com/tootsuite/mastodon).

## Assets
All images, videos, and other files you can upload to the instance are hosted on **[DigitalOcean Spaces](https://www.digitalocean.com/products/spaces/)**. The bucket (the location containing all assets) is hosted in the `fra1` region, also located in Frankfurt, Germany. A [CDN (Content Delivery Network)](https://www.digitalocean.com/docs/spaces/#cdn), a network of edge servers that deliver content to users, is enabled for our assets. This lets users download images from a server closer to them instead of downloading images/videos from Germany. It uses `assets.merveilles.town` as the domain.

## Emails
Email notifications sent from `notifications@email.merveilles.town`, used for password resets and the like, are sent through **[Mailgun](https://www.mailgun.com/)**.
We also have an account and receive emails at **[Fastmail](https://fastmail.com/)** for our various services.

## Domain Registration
The [merveilles.town](https://merveilles.town) domain is registered through **[Gandi](https://gandi.net/)** and is set to automatically renew before the registration expires.

## Funding
We currently use a **[Patreon](https://patreon.com/merveillestown)** so we can get donations to keep the server running. We're investigating other options to let people donate in other ways as well.

# Cost
| Resource | Cost | Reference | Notes |
|----------|------|-----------|-------|
| DigitalOcean Droplet | **USD$40.00** per month | [https://www.digitalocean.com/pricing/](https://www.digitalocean.com/pricing/) | Droplets can be cheap, but Mastodon specifically needs at least 4 gigabytes of memory to compile assets and Elasticsearch, which powers full-text search, needs more on top of that. |
| DigitalOcean Droplet Backups | **USD$8.00** per month | [https://www.digitalocean.com/pricing/](https://www.digitalocean.com/pricing/) | Having backups enabled costs an extra 20% of the droplet cost. |
| DigitalOcean Spaces | **USD$5.00** per month | [https://www.digitalocean.com/pricing/#Storage](https://www.digitalocean.com/pricing/#Storage) | We should be able to comfortably stay on the $5 tier at our current size, especially with the [CDN](https://www.digitalocean.com/docs/spaces/#cdn) enabled. |
| Gandi Registration | **USD $3.43** per month / **USD$41.21** per year | [https://www.gandi.net/en/domain/tld/town](https://www.gandi.net/en/domain/tld/town) | The `.town` top-level domain (TLD) is a bit more expensive to register than other TLDs. We pay this on a yearly basis. |
| Fastmail | **USD$4.17** per month / **USD$50** per year | [https://www.fastmail.com/pricing/](https://www.fastmail.com/pricing/) | We need to have the Standard tier so that we can use our own domain. We pay for the yearly license to save on costs. |
| Mailgun | **USD$0.00** per month | [https://www.mailgun.com/pricing/](https://www.mailgun.com/pricing/) |  The free tier allows for 10000 emails sent per month, which we should be able to comfortably stay below with a smaller number of users. |

## Total Cost
About **USD$60.60 per month**. This cost can be higher based on the amount of resources on Spaces that the instance uses. This estimated cost does not cover any unexpected charges.
