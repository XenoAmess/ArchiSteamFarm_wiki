# Docker

Starting with version 3.0.3.2, ASF is now also available as Docker container. Our repo can be found **[here](https://hub.docker.com/r/justarchi/archisteamfarm)**.

---

## Tags

ASF is available through 3 main types of tags:


### `master`

This tag always points to the ASF built from latest commit in master branch, which works the same as experimental AppVeyor build described in our **[release cycle](https://github.com/JustArchi/ArchiSteamFarm/wiki/Release-cycle)**. Typically you should avoid this tag, as it's the highest level of bugged software dedicated to developers and advanced users for development purposes.


### `latest`

This tag always points to the latest **[released](https://github.com/JustArchi/ArchiSteamFarm/releases)** ASF version, including pre-releases. It's the default and therefore recommended tag for grabbing latest ASF version for usage in Docker container.

### `A.B.C.D`

In addition to `master` and `latest` tags that will typically change with time and always point to appropriate releases, ASF docker repo also provides you with fixed frozen release that you can use if you want to have specific version installed. This version, once built, is considered frozen and will not be modified in any way, just like our GitHub releases.

---

## Usage