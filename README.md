# MirrorZ

Your next MirrorS is not MirrorS, nor MirrorSes, it's MirrorZ.

## Demo

[https://www.mirrorz.org](https://www.mirrorz.org). Currently `mirrorz.json` for each MirrorS is pre-generated, not real-time generated.

## Intro

MirrorS are heterogeneous. It is hard for a single mirror to provide all mirrors, so differences occur.

For end users, this is not a good experience as they need to search for available mirrors.

To make things easy, MirrorZ is intended to include all mirrorS, so a unified interface is needed.

## Data Format v1 (draft)

Each MirrorS participating in MirrorZ should provide a `mirrorz.json` with the following fields.

```json
{
  "version": 1,
  "site": {
    "url": "https://example.org",
    "logo": "https://example.org/favicon.ico",
    "abbr": "EXAMPLE",
  },
  "info": [
    {
      "distro": "Debian",
      "category": "os",
            "urls": [
                {
                    "name": "10.7.0 (amd64, CD installer with xfce)",
                    "url": "/debian-cd/current/amd64/iso-cd/debian-10.7.0-amd64-xfce-CD-1.iso"
                }
            ]
    }
  ],
  "mirrors": [
    {
      "cname": "AOSP",
      "desc": "Android 操作系统源代码",
      "url": "/AOSP",
      "status": "S",
      "help": "/help/AOSP/",
      "upstream": "https://android.googlesource.com/mirror/manifest"
    },
    {
      "cname": "AUR",
      "desc": "Arch Linux 用户软件库",
      "url": "https://aur.tuna.tsinghua.edu.cn/",
      "status": "S",
      "help": "/help/AUR/",
      "upstream": "https://aur.archlinux.org/"
    }
  ]
}
```

### Notes

* `version` is optional for version 1
* `site` provides the global info about one MirrorS
* `site.url` should not end with slash `/`
* `info` is used for category view
* the name of `info.distro` should be agreed and have a mapping, maintained in `cname.json`
* `mirrors` is used for list view
* the name of `mirrors.cname` should be agreed and have a mapping, maintained in `cname.json`
* `mirrors.desc` may differ for each MirrorS since there are `excludes` for some MirrorS
* `mirrors.desc` may be empty
* if `mirrors.url` begins with a slash `/`, it should be appended to `site.url` to form a full url
* `mirrors.status` should be agreed, currently some values are defined in `tunasync`
* `mirrors.help` may be empty, or the same rule as `mirrors.url`
* `mirrors.upstream` may be empty

## Backend and Frontend

For each MirrorS, it should provide a json file of the above format (recommended name `mirrorz.json`) and allow CORS of `www.mirrorz.org` on that file (CORS on all domains is recommended as some other sites may also use this; and useful for debuging).

MirrorS may provide `mirrorz.json` using their mirror servers, or any other valid url that reflects the real-time status of their mirror. For example, for TUNA-series MirrorS, a CloudFlare worker is deployed (currently another server `status.tuna.wiki` is used instead of CF worker as it is slow on generating json file).

The list of participating MirrorS should be maintained in `_include/mirrors.json`.

For the front end, currently a naive one is implemented using JQuery, one MirrorS may provide their FrontEnd for rendering, for example `www.mirrorz.org/{tuna,ustc,sjtug,hit}/index.html`.

`www.mirrorz.org` should only be statically generated.

With user's consent, `www.mirrorz.org` may use Cookie or other methods to archive personalized rendering such as turning off some MirrorS when the json file is large and/or slow to download.

## Contributing and Developing

### MirrorS

For participating MirrorS, it should add and maintain their url to mirrorz json file in `_include/mirrors.json`, in the url `https` is needed, and the url should be widely accessible (not limited to campus) as all users of `mirrorz.org` would request that url.

MirrorS may also contribute their `mirrorz.json` generating scripts in the directory `scripts`. The standard script is `scripts/tunasync/mirrorz.py`, all the details are specified there and unclear points of the data format is explaned there.

If one adds directory, the directory should be recorded in the exclude list of `_config.yml`.

### Frontend

Currently `jekyll` is used. The installation of jekyll is not documented here. One may use

```
jekyll s
```

to start a local server.

For other frontend, like tuna-stylish frontend, they should be added using permlink like `/tuna/`.

### Misc

Currently `*.json` and `*.xml` is ignored in `.gitignore`, but not excluded in `_config.yml`. If one wants to commit a json file, they should use `git add -f`.

<!--
 vim: ts=2 sts=2 sw=2
-->
