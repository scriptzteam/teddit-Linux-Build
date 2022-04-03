# teddit-Linux-Build

[teddit.net](https://teddit.net)

[reddit.0xc0d3.xyz](https://reddit.0xc0d3.xyz/)

A free and open source alternative Reddit front-end focused on privacy.
Inspired by the [Nitter](https://github.com/zedeus/nitter) project.

* No JavaScript or ads
* All requests go through the backend, client never talks to Reddit
* Prevents Reddit from tracking your IP or JavaScript fingerprint
* [Unofficial API](https://codeberg.org/teddit/teddit/wiki#teddit-api) (RSS & JSON support, no rate limits or Reddit account required)
* Lightweight (teddit frontpage: ~30 HTTP requests with ~270 KB of data downloaded vs. Reddit frontpage: ~190 requests with ~24 MB)
* Self-hostable. Anyone can setup an instance. An instance can either use Reddit's API with or without OAuth (so Reddit API key is not necessarily needed).

Join the teddit discussion room on Matrix: [#teddit:matrix.org](https://matrix.to/#/#teddit:matrix.org)

XMR: 832ogRwuoSs2JGYg7wJTqshidK7dErgNdfpenQ9dzMghNXQTJRby1xGbqC3gW3GAifRM9E84J91VdMZRjoSJ32nkAZnaCEj

## Instances

[https://teddit.net](https://teddit.net) - Official instance

Community instances:

| Instance | Onion Link | I2P | Notes |
|-|-|-|-|
| [reddit.0xc0d3.xyz](reddit.0xc0d3.xyz) |  |  | |
| [teddit.ggc-project.de](https://teddit.ggc-project.de) |  |  | |
| [teddit.zaggy.nl](https://teddit.zaggy.nl) |  |  | |
| [teddit.namazso.eu](https://teddit.namazso.eu) |  |  | |
| [teddit.nautolan.racing](https://teddit.nautolan.racing) |  |  | |
| [teddit.tinfoil-hat.net](https://teddit.tinfoil-hat.net) |  |  | |
| [teddit.domain.glass](https://teddit.domain.glass) |  |  | |
| [snoo.ioens.is](https://snoo.ioens.is) | [snoo.ioensistjs7wd746...onion](http://snoo.ioensistjs7wd746zluwixvojbbkxhr37lepdvwtdfeav673o64iflqd.onion/) |  | |
| [teddit.httpjames.space](https://teddit.httpjames.space) |  |  | |
|  | [ibarajztopxnuhabfu7f...onion](http://ibarajztopxnuhabfu7fg6gbudynxofbnmvis3ltj6lfx47b6fhrd5qd.onion) | [xugoqcf2pftm76vbznx4...i2p](http://xugoqcf2pftm76vbznx4xuhrzyb5b6zwpizpnw2hysexjdn5l2tq.b32.i2p) | Operated by [mdleom.com](https://mdleom.com/about/#Services) |
| [teddit.alefvanoon.xyz](https://teddit.alefvanoon.xyz) |  |  | |
| [incogsnoo.com](https://incogsnoo.com) | [tedditfyn6idalzso5wam....onion](tedditfyn6idalzso5wam5qd3kdtxoljjhbrbbx34q2xkcisvshuytad.onion/) | [http://teddit.i2p](http://teddit.i2p) | |
| [teddit.pussthecat.org](https://teddit.pussthecat.org) |  |  | Operated by [PussTheCat.org](https://pussthecat.org/) |
| [reddit.lol](https://reddit.lol) | [http://dawtyi5e2cfyfmoht...onion](http://dawtyi5e2cfyfmoht4izmczi42aa2zwh6wi34zwvc6rzf2acpxhrcrad.onion) | [http://vzeiwzi7ogwl3i...b32.i2p](vzeiwzi7ogwl3ijrfek4fbtwhvamxcpyqoc3s4vcgnhlp54s5clq.b32.i2p) | Operated by https://liberta.casa | |
| [teddit.sethforprivacy.com](https://teddit.sethforprivacy.com/) | [qtpvyiaqhmwccx...onion/](http://qtpvyiaqhmwccxwzsqubd23xhmmrt75tdyw35kp43w4hvamsgl3x27ad.onion/) |  | For more similar hosted tools, see [blog.sethforprivacy.com](https://blog.sethforprivacy.com/about/#my-community-resources) |
| [teddit.totaldarkness.net](https://teddit.totaldarkness.net) |  |  | |
| [teddit.adminforge.de](https://teddit.adminforge.de) |  | | Operated by https://adminforge.de |
| [teddit.bus-hit.me](https://teddit.bus-hit.me) |  | | Operated by https://bus-hit.me |
| [teddit.froth.zone](https://teddit.froth.zone) |  | | |
| [rdt.trom.tf](https://rdt.trom.tf) |  | | Part of the https://trom.tf project |
<!--
Remove the Changelog section, because the CHANGELOG.md is not updated anymore
## Changelog

See ```CHANGELOG.md```
-->

## Installation

### Docker-compose method (production)

```docker
version: "3.8"

services:

  teddit:
    container_name: teddit
    image: teddit/teddit:latest
    environment:
      - DOMAIN=teddit.net
      - USE_HELMET=true
      - USE_HELMET_HSTS=true
      - TRUST_PROXY=true
      - REDIS_HOST=teddit-redis
    ports:
      - "127.0.0.1:8080:8080"
    networks:
      - teddit_net
    healthcheck:
      test: ["CMD", "wget" ,"--no-verbose", "--tries=1", "--spider", "http://localhost:8080/about"]
      interval: 1m
      timeout: 3s
    depends_on:
      - teddit-redis

  teddit-redis:
    container_name: teddit-redis
    image: redis:6.2.5-alpine
    command: redis-server
    environment:
      - REDIS_REPLICATION_MODE=master
    networks:
      - teddit_net

networks:
  teddit_net:
```

Note: This compose is made for a true "production" setup, and is made to be used to have teddit behind a reverse proxy, if you don't want that and prefer to directly access teddit via its port:

- Change `ports: - "127.0.0.1:8080:8080"` to `ports: - "8080:8080"`
- Remove `DOMAIN=teddit.net`, `USE_HELMET=true`, `USE_HELMET_HSTS=true`, `TRUST_PROXY=true`


### Docker-compose method (development)

```bash
git clone https://codeberg.org/teddit/teddit
cd teddit
docker-compose build
docker-compose up
```

Teddit should now be running at <http://localhost:8080>.

Docker image is available at [https://hub.docker.com/r/teddit/teddit](https://hub.docker.com/r/teddit/teddit).

#### Environment Variables

The following variables may be set to customize your deployment at runtime.

| Variable | Description |
|-|-|
| domain | Defines URL for Teddit to use (i.e. teddit.domain.com). Defaults to **127.0.0.1** |
| use_reddit_oauth | *Boolean* If true, "reddit_app_id" must be set with your own Reddit app ID. If false, Teddit uses Reddit's public API. Defaults to **false** |
| cert_dir | Defines location of certificates if using HTTPS (i.e. /home/teddit/le/live/teddit.net). No trailing slash. |
| theme | Automatically theme the user's browser experience. Options are *auto*, *dark*, *sepia*, or you can set *white* by setting the variable to empty ( '' ). Defaults to **auto** |
| flairs_enabled | Enables the rendering of user and link flairs on Teddit. Defaults to **true** |
| highlight_controversial | Enables controversial comments to be indicated by a typographical dagger (†). Defaults to **true** |
| api_enabled | Teddit API feature. Might increase loads significantly on your instance. Defaults to **true** |
| api_force_https | Force HTTPS to Teddit API permalinks (see #285). Defaults to **false** |
| video_enabled | Enables video playback within Teddit. Defaults to **true** |
| redis_enabled | Enables Redis caching. If disabled, does not allow for any caching of Reddit API calls. Defaults to **true** |
| redis_db | Sets the redis DB name, if required |
| redis_host | Sets the redis host location, if required. Defaults to **127.0.0.1** |
| redis_password | Sets the redis password, if required |
| redis_port | Sets the redis port, if required. Defaults to **6379** |
| ssl_port | Sets the SSL port Teddit listens on. Defaults to **8088** |
| nonssl_port | Sets the non-SSL port Teddit listens on. Defaults to **8080** |
| listen_address | Sets the address Teddit listens for requests on. Defaults to **0.0.0.0** |
| https_enabled | *Boolean* Sets whether or not to enable HTTPS for Teddit. Defaults to **false** |
| redirect_http_to_https | *Boolean* Sets whether to force redirection from HTTP to HTTPS. Defaults to **false** |
| redirect_www | *Boolean* Redirects from www to non-www URL. For example, if true, Teddit will redirect https://www.teddit.com to https://teddit.com. Defaults to **false** |
| use_compression | *Boolean* If set to true, Teddit will use the [https://github.com/expressjs/compression](Node.js compression middleware) to compress HTTP requests with deflate/gzip. Defaults to **true** |
| use_view_cache | *Boolean* If this is set to true, view template compilation caching is enabled. Defaults to **false** |
| use_helmet | *Boolean* Recommended to be true when using https. Defaults to **false** |
| use_helmet_hsts | *Boolean* Recommended to be true when using https. Defaults to **false** |
| trust_proxy | *Boolean* Enable trust_proxy if you are using a reverse proxy like nginx or traefik. Defaults to **false** |
| trust_proxy_address | Location of trust_proxy. Defaults to **127.0.0.1** |
| http_proxy | Set http/https proxy to use for outgoing requests. See [https-proxy-agent](https://github.com/TooTallNate/node-https-proxy-agent) for details |
| nsfw_enabled | *Boolean* Enable NSFW (over 18) content. If false, a warning is shown to the user before opening any NSFW post. When the NFSW content is disabled, NSFW posts are hidden from subreddits and from user page feeds. Note: Users can set this to true or false from their preferences. Defaults to **true** |
| videos_muted | *Boolean* Automatically mute all videos in posts. Defaults to **true** |
| post_comments_sort | Defines default sort preference. Options are *confidence* (default sorting option in Reddit), *top*, *new*, *controversal*, *old*, *random*, *qa*, *live*. Defaults to **confidence** |
| reddit_app_id | If "use_reddit_oauth" config key is set to true, you have to obtain your Reddit app ID. For testing purposes it's okay to use this project's default app ID. Create your Reddit app here: https://old.reddit.com/prefs/apps/. Make sure to create an "installed app" type of app. Default is **ABfYqdDc9qPh1w** |
| domain_replacements | Replacements for domains in outgoing links. Tuples with regular expressions to match, and replacement values. This is in addition to user-level configuration of privacyDomains. Defaults to **[]** |
| cache_control | *Boolean* If true, teddit will automatically remove all cached static files. Defaults to **true** |
| cache_control_interval | How often the cache directory for static files is emptied (in hours). Default is every 24 hours. Requires cache_control to be true. Defaults to **24** |

### Manual

1. Install [Node.js](https://nodejs.org).

1. (Optional) Install [redis-server](https://redis.io).

   Highly recommended – it works as a cache for Reddit API calls.

1. (Optional) Install [ffmpeg](https://ffmpeg.org).

   It's needed if you want to support videos.

   ```bash
   # Linux
   apt install redis-server ffmpeg

   # macOS
   brew install redis
   ```

1. Clone and set up the repository.

   ```bash
   git clone https://codeberg.org/teddit/teddit
   cd teddit
   npm install --no-optional
   cp config.js.template config.js # edit the file to suit your environment
   redis-server
   npm start
   ```

Teddit should now be running at <http://localhost:8080>.
