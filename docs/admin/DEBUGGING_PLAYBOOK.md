# Merveilles.town Debugging Playbook

So, something's gone wrong on the instance. Seeing lots of 500 errors? Not able to connect to the Mastodon instance at all? This document should go over the available dashboards we have, how to try and see what's going on, and then how to resolve those errors when they come up.

## Before you start

You'll need to have access to the **Merveilles 1Password vault**, **SSH access to the VPS** our instance is hosted on, and **administrator permissions** on https://merveilles.town. If you are an administrator and do not have access to these resources, contact [Somnius](https://merveilles.town/@somnius) and they'll get you set up.

## Dashboards, Alerting, and Other Tools

We have a few different places we can look to see what's going on, and tools we can use to mitigate issues:

- Mastodon Admin Panels on https://merveilles.town
	- Sidekiq
	- PGHero
- DigitalOcean alerting & graphs
- Tools available when SSH'd into the VPS
	- `htop`
	- Server Logs
	- Restarting processes
	- Reboot the instance

Let's go over these in detail.

### Mastodon Admin Panels

These administrative panels are available when you are signed into an administrator account on the Mastodon droplet. To get to these, sign into https://merveilles.town with your administrator account and head to: https://merveilles.town/admin/dashboard

Here, you'll see the site settings, announcements, and other tools available only to administrators, but towards the bottom of the Admin section, you'll see two links: Sidekiq and PgHero.

<img width="200" alt="xpanded admin view, highlighting Sidekiq and PgHero links" src="screenshots/Screen Shot 2020-09-25 at 21.31.06.png" />

#### Sidekiq

This is a tool used with Ruby web apps (and Ruby on Rails apps) that handles background processing. On our instance, for example, this tool handles crawling links to get meta previews, sends web notifications, and handles sending toots/posts to other instances, automatically retries accessing posts on other instances, and quite a lot more besides.

When you click on the "Sidekiq" link under "Administration", you should see a dashboard like this:

<img width="100%" alt="Sidekiq dashboard" src="screenshots/Screen Shot 2020-09-25 at 21.34.07.png" />

This will show you the amount of background processes that have succeeded and failed. You should generally see a whole lot more successes than failures, but because of how Mastodon works, with instances being put up and taken down all the time, there will always be a small percentage of failed processes.

Keep an eye on the proportion of successful vs failed processes. If the amount of failed processes gets too high, that generally means something wrong is happening.

#### PgHero

This is a good place to look in case there are any database issues.

Mastodon uses Postgres for its database, and PgHero is a tool that lets you monitor various information about the database, including how big it is, how many concurrent connections are being made to the database, or any other issues related to that.

When you click on the PgHero link, you should see a page that looks like this: 

<img width="100%" alt="PgHero dashboard" src="screenshots/Screen Shot 2020-09-25 at 21.52.25.png" />

### DigitalOcean

We are running on a DigitalOcean "Droplet" (their fancy techbro word for a Virtual Private Server / VPS) which has quite a few features available. In particular, we can take a look at its graphs (showing RAM usage, hard drive usage, etc) and set up automatic alerting.

#### Graphs
Log into DigitalOcean with the account in 1Password, and click on the droplet named `THE-TOWN`. You should be taken to a page that looks like this:

<img width="100%" alt="DigitalOcean droplet dashboard" src="screenshots/Screen Shot 2020-09-25 at 22.51.33.png" />

This will allow you to see how our server is currently doing, and will allow you to see the the changes in that information up to 14 days in the past.

When looking at this dashboard, keep an eye out for drastic changes that look unusual. Has there been a huge, sustained spike in bandwidth? Has the memory usage suddenly increased or decreased? If so, that might help tell us where something is going wrong.

Note: Memory usage is the key graph to look at here. When left alone, memory usage starts at around 30-40% and will continue to increase until it maxes out and hovers at about 80%. This is because of the all the processes running concurrently (in particular, Elasticsearch is quite memory-hungry). When you restart all Mastodon processes, the usage will drop back down to around 30-40% again.

#### Monitoring/Alerts

The other utility of having a DigitalOcean droplet is that you can configure automatic alerts. These alerts will fire when certain thresholds are met for a particular metric (for example, for memory usage, or disk read/write).

We already have alerts set up for all available metrics. You can see and adjust these by clicking on "Monitoring" on the left-hand panel on DigitalOcean. You should see a page like this:

<img width="100%" alt="DigitalOcean configured monitors" src="screenshots/Screen Shot 2020-09-25 at 23.07.52.png" />

### Tools on the VPS

These tools are available on the VPS/Droplet itself. To get there, SSH into the VPS by running `ssh root@merveilles.town`. Once you do, you should see a screen that looks like this:

<img width="100%" alt="Console output after SSH'ing into merveilles.town" src="screenshots/Screen Shot 2020-09-25 at 23.17.44.png" />

Now, you can look through the server itself to see what's up, and you can restart processes as needed. Let's go over the exact tools.

#### htop

`htop` is a tool that lets you see the current resource usage of your server. Open it up by simply running `htop`, and you should see this:

<img width="100%" alt="Console output of htop, showing processes and current resource usage" src="screenshots/Screen Shot 2020-09-25 at 23.21.54.png" />

The way DigitalOcean calculates these metrics is slightly different from how `htop` does it, so you may see a larger amount of used memory here. However, you can use this to see what particular process is using the most memory, CPU, and whatever else. If there's a need to restart a particular process, you can also look at the PID on the left-hand side to target that particular process.

This is useful because you're able to see how the server is immediately doing.

#### Server Logs

This is the most difficult thing to sift through, but it can be the most useful because it can *tell you exactly what caused a particular problem*. Essentially, you can search through our various logs to see what, when, or how a particular issue arose.

Most processes will log to `sysctl/journalctl`, including all Mastodon-prefixed processes. However, Elasticsearch does not, and houses logs in its own folder.

To search through server logs for non-Elasticsearch processes, here's an example command:

```
journalctl -u mastodon-sidekiq.service -S "2020-06-19 23:30:00"
```

This opens up `journalctl`, showing logs only related to Sidekiq, at that particular time. You can also `grep` for specific keywords:

```
journalctl -u mastodon-sidekiq.service | grep -i error
```

This looks through the `journalctl` logs for Sidekiq, specifically looking for all instances of the keyword "error".

To look through the Elasticsearch logs, you can do this:

```
grep -ir 'error' /var/log/elasticsearch
```

This looks through the Elasticsearch log folder, looking for the "error" keyword.

Definitely look through the documentation for `journalctl`, `grep`, and Elasticsearch to see how to use these properly—they all have very useful features that won't be touched on in this document just because there's too much. However, looking through here can help diagnose when or why a certain issue occurred.

#### Restarting Processes

In most cases, the solution to a given problem is simply to restart the relevant process. See an issue with Sidekiq? Restart Sidekiq. Elasticsearch not working? Restart it. Here are the commands you can run to restart all four main processes:

```
systemctl restart mastodon-web
systemctl restart mastodon-streaming
systemctl restart mastodon-sidekiq
systemctl restart elasticsearch
```

Restarting these processes is *strongly preferable* to rebooting the server outright, because it minimizes downtime. While Sidekiq is taking the time to reboot, our users are still able to access the posts that the instance has retrieved, though it might take a bit to send a reply to another instance, it will get picked up when Sidekiq is up and running again.

#### Rebooting the server

If all else fails and there are no other options, we can reboot the server outright. However, **make sure there is no other option first, and that you give our users a bit of warning before doing so**. Rebooting the server causes the instance to become completely inaccessible and it takes several minutes for it to come back online again.

To reboot the server, it's a very simple command—just type this in and press Enter:
```
reboot
```

You will not be able to access the instance while it is rebooting and you will be kicked out of your SSH session.

## Common Issues

These are a few example situations that have happened before.

- **Problem**: Sidekiq is stuck on a process and keeps endlessly retrying, preventing other processes from succeeding.
	- **Solution**: Delete that process. It may be in "Busy", "Retries", or "Scheduled", but in all three, you can select a particular process and kill it.
	<img width="100%" alt="Shows how to select a process by clicking the checkbox on the left" src="screenshots/Screen Shot 2020-09-25 at 21.43.02.png" />
	<img width="100%" alt="Shows how to kill a process by clicking the 'kill' button" src="screenshots/Screen Shot 2020-09-25 at 21.44.28.png" />

- **Problem**: The Elasticsearch process has stopped, causing several jobs in Sidekiq to fail.
	- **Solution**: Restart Elasticsearch and Sidekiq. 
	```
	systemctl restart elasticsearch
	systemctl restart sidekiq
	```
