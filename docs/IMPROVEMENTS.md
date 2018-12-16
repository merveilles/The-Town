# Improvements

This is an outline of possible improvements and goals for the [Town of Merveilles](https://merveilles.town). These goals may change over time, and not all of these improvements will be feasible.

# Low-Effort Improvements

## Customization

* Adjust hero image, mascot, and the instance thumbnail.
* Use CSS to give the instance a dark theme.

## Version control

Ensure that all sitewide text (terms of service, extended info, CSS) are in version control, including:

* Short instance description
* Instance description
* Closed registration message
* Custom extended information
* Custom terms of service
* Custom CSS

# Mid-Effort Improvements

## Custom UI Icons
Replace icons and use custom ones:
* Replies
* Boosts
* Favorites

## Federation Relay
Enable a Federation Relay server to help with discovery.

## Managing Donations
* Create a Patreon for people to optionally donate, mitigating server costs.
* Ensure and enforce a system so that, if the incoming donations go over the monthly usage cost, the extra money is still being used appropriately. For example, that money can be redistributed back to Merveilles members to finance their projects, or can be donated to a chosen charity.

# High-Effort Improvements

## Infrastructure Upgrades
* **CDN**: Enable a Cloud Distribution Network with S3, so that users all over the world can load assets faster.
* **Load Balancing**: Create multiple connected droplets on DigitalOcean and do load balancing between them, for better site reliability.
* **Move to docker**: Moving to [docker](https://www.docker.com/) would be much easier to manage. Using docker, each part of Mastodon can be put inside its own container, which is easier to track for logging purposes, and would be way easier to spin up and take down as needed. It may be a bit too automagical, however.
