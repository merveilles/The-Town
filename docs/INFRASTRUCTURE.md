# Infrastructure

In an effort to be transparent and in the spirit of collaboration, this document outlines the infrastructure and recurring cost of the [Town of Merveilles](https://merveilles.town).

## Staff

- **Owners** (Own the instance itself)
  - [Somnius](https://merveilles.town/@somnius)
  - [Devine Lu Linvega](https://merveilles.town/@neauoire)
- **Administrators** (Have access to the server infrastructure)
  - [Flip](https://merveilles.town/@flip)
- **Moderators** (Have control over federation & invites)
  - [Öppen](https://merveilles.town/@oppen)
  - [Glyph](https://merveilles.town/@glyph)

## Server
Merveilles.town is an instance of **[Hometown](https://github.com/hometown-fork/hometown) version v1.1.1+4.2.10**, forked from **[Mastodon](https://joinmastodon.org) v3.5.2**, that is on a virtual private server (VPS), or "droplet", hosted on **[DigitalOcean](http://digitalocean.com/)**. The VPS is hosted in **DigitalOcean**'s `FRA1` region, located in Frankfurt, Germany. It is a manual install, done by going through the [installation instructions](https://docs.joinmastodon.org/admin/install/) outlined in the [documentation](https://docs.joinmastodon.org/). The VPS operating system is **64-bit [Ubuntu](https://www.ubuntu.com/) version 18.04 LTS**, created using **DigitalOcean**'s one-click install. To see the tools powering **Mastodon**, you can reference its [GitHub Page](https://github.com/tootsuite/mastodon) and [documentation](https://docs.joinmastodon.org).

## Assets
All images, videos, and other files you can upload to the instance are hosted on **[DigitalOcean Spaces](https://www.digitalocean.com/products/spaces/)**. The bucket (the location containing all assets) is hosted in the `fra1` region, also located in Frankfurt, Germany. A [CDN (Content Delivery Network)](https://www.digitalocean.com/docs/spaces/#cdn), a network of edge servers that deliver content to users, is enabled for our assets. This lets users download our assets from a server closer to them instead of having to download from the bucket in Germany. It uses `assets.merveilles.town` as the domain.

## Emails
Email notifications sent from `notifications@email.merveilles.town`, used for password resets and the like, are sent through **[Mailgun](https://www.mailgun.com/)**.
We also have our own email account through **[Fastmail](https://fastmail.com/)**, which the staff uses to send invites and receive emails for the services the instance uses.

## Domain Registration
The [merveilles.town](https://merveilles.town) domain is registered through **[Gandi](https://gandi.net/)** and is set to automatically renew before the registration expires.

## Funding
We have multiple sources of funding so we can get donations to keep the server running, and a monthly financial report is made by [Somnius](https://merveilles.town/@somnius) to show how money is both spent and received. To see how much money we're receiving, please visit those respective resources below:
- **[Patreon](https://patreon.com/merveillestown)**
- **[Ko-Fi](https://ko-fi.com/merveillestown)**
- **[OpenCollective](https://opencollective.com/merveilles)**
- **[Liberapay](https://liberapay.com/merveilles)**

# Cost
| Resource | Cost | Reference | Notes |
|----------|------|-----------|-------|
| DigitalOcean Droplet | **USD$112.00** per month | [https://www.digitalocean.com/pricing/droplets](https://www.digitalocean.com/pricing/droplets) | We use the 8 vCPU, 16GB of RAM size for our droplet. Mastodon specifically needs at least 4 gigabytes of memory to compile assets, and Elasticsearch (which powers full-text search) needs at least a gigabyte of memory as well. We sized up a while ago since we were running into RAM limits with the influx in Mastodon traffic. |
| DigitalOcean Droplet Backups | **USD$8.00** per month | [https://www.digitalocean.com/pricing/](https://www.digitalocean.com/pricing/) | Enabling backups adds 20% to the cost of the droplet, but is necessary in case something implodes. |
| DigitalOcean Spaces | **USD$5.00** per month | [https://www.digitalocean.com/pricing/spaces-object-storage](https://www.digitalocean.com/pricing/spaces-object-storage) | We should be able to comfortably stay on the $5 tier at our current size, especially with the [CDN](https://www.digitalocean.com/docs/spaces/#cdn) enabled. |
| Gandi Registration | **USD $3.43** per month / **USD$41.21** per year | [https://www.gandi.net/en/domain/tld/town](https://www.gandi.net/en/domain/tld/town) | The `.town` top-level domain (TLD) is a bit more expensive to register than other TLDs. We pay this on a yearly basis. |
| Fastmail | **USD$4.17** per month / **USD$50** per year | [https://www.fastmail.com/pricing/](https://www.fastmail.com/pricing/) | We need to have the Standard tier so that we can use our own domain. We pay for the yearly license to save on costs. |
| Mailgun | **USD$0.00-2.00** per month | [https://www.mailgun.com/pricing/](https://www.mailgun.com/pricing/) |  The flex tier allows for 1000 emails sent per month, but we've been seeing an increase in usage recently, which has been somewhere above USD$1. |

## Total Cost: _USD$134.60 per month_.
This cost can be higher based on the amount of resources on Spaces that the instance uses. This estimated cost does not cover any unexpected charges.
