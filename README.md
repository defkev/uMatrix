> [!NOTE]  
> This fork adds support to properly control 3rd-party referrer-spoofing (essential implementing https://github.com/uBlockOrigin/uMatrix-issues/issues/171), mostly to unbreak embedded YouTube, which throws an arbitrary error (4 or 15) when the referrer is missing or spoofed as www.youtube.com, normally requiring disabling spoofing for the whole site or globally to unbreak.

It adds a new default rule `* first-party referrer allow` which is mimicking the old `referrer-spoof: false` for 1st party requests.

This completely removes support for the legacy `referrer-spoof` key, including the default `referrer-spoof: behind-the-scene false` which is now handled by the existing `matrix-off: behind-the-scene true`

If you have any custom `referrer-spoof` rules in your config, you need to manually migrate them to the new syntax, e.g. `referrer-spoof: example.com true` becomes `example.com * referrer block` and `referrer-spoof: www.example.com false` becomes `www.example.com * referrer allow`

There is currently no code to migrate a legacy config to the new syntax, so if you have many `referrer-spoof` rules, good luck!

> [!WARNING]
> Starting with version `1.5.4` uMatrix for **Firefox** will isolate temporary rules from private (incognito) in non-private (normal) windows and vice-versa (https://github.com/gorhill/uMatrix/issues/350) to prevent exceptions added for domains visited in private windows from leaking into the normal browser session.

Temporary rules can still be committed in either private or non-private windows and then restored in the other.

Temporary rules in private windows will be cleared the moment the last private window gets closed.

To restore the old (insecure) behavior of sharing temporary rulesets between windows, set `isolateMatrix` to `false` under advanced settings and restart uMatrix.

**This feature is currently only available in Firefox**

> [!TIP]
> A signed Firefox XPI and packed Chrome ZIP can be downloaded under releases (on the right ->)

There is no PR because the original repo was archived a long time ago.
Furthermore, this is not on AMO and probably never will be because the site is run by a bunch of technologically illiterate retards and I don't have time to play customer support for people who cannot follow a simple README but insist on reviewing other people's code they don't understand.
The fork also isn't on CWS (Chrome Web Store) because uMatrix heavily relies on APIs (still) not available in MV3 (Manifest Version 3) and Google no longer accepting MV2 submissions.

If you have uMatrix already installed, you cannot update using this version since they have been signed with different keys. In this case, create a backup under `Settings > About > Your data > Back up to file...` uninstall gorhill's version, install this one, and restore your config using `Settings > About > Your data > Restore from file...`. Again, if you have any custom `referrer-spoof` rules write them down first!

## uMatrix<br>[<img src="https://travis-ci.org/gorhill/uMatrix.svg?branch=master" height="16">](https://travis-ci.org/gorhill/uMatrix)

Definitely for advanced users.

***

Keep Github issues for actual bugs. User support is [/r/uMatrix](https://www.reddit.com/r/uMatrix/).

Forked and refactored from [HTTP Switchboard](https://github.com/gorhill/httpswitchboard).

Install [manually](https://github.com/gorhill/uMatrix/blob/master/doc/README.md) the [latest release](https://github.com/gorhill/uMatrix/releases), or install from:
- [Firefox AMO](https://addons.mozilla.org/firefox/addon/umatrix/)
    - To help find issues with ongoing development: [uMatrix dev build in _Releases_](https://github.com/gorhill/uMatrix/releases) (click the latest `uMatrix.webext.signed.xpi` link of the last pre-resease.)
- [Chrome store](https://chrome.google.com/webstore/detail/µmatrix/ogfcmafjalglgifnmanfmnieipoejdcf)
    - To help find issues with ongoing development: [uMatrix dev build in Chrome store](https://chrome.google.com/webstore/detail/umatrix-dev-build/eckgcipdkhcfghnmincccnhpdmnbefki)
- [Opera store](https://addons.opera.com/en-gb/extensions/details/umatrix/)

You may contribute with translation work:
- For in-app strings, on Crowdin: [uMatrix on Crowdin](https://crowdin.com/project/umatrix).
- For [description](https://github.com/gorhill/uMatrix/tree/master/doc/description) (to be used in AMO, Chrome store, etc.), submit a pull request. [Reference description is here](https://github.com/gorhill/uMatrix/blob/master/doc/description/description.txt) (feel free to improve as you wish, I am not a writer).

[HTTP Switchboard's documentation](https://github.com/gorhill/httpswitchboard/wiki) is still relevant, except for [uMatrix's differences with HTTP Switchboard](https://github.com/gorhill/uMatrix/wiki/Changes-from-HTTP-Switchboard).

You may contribute with documentation: [uMatrix's wiki](https://github.com/gorhill/uMatrix/wiki).

## Warnings

#### Regarding broken sites

uMatrix does not guarantee that sites will work fine: it is for advanced users who can figure how to un-break sites, because essentially uMatrix is a firewall which works in relaxed block-all/allow-exceptionally mode out of the box: it is not unexpected that sites will break.

**So this means do not file issues to report broken sites when the sites are broken because uMatrix does its job as expected.** I will close any such issue without further comment.

**Using uMatrix logger is key to un-break sites:** the logger will show you all that uMatrix does internally.

I expect there will be community driven efforts for users to help each others. If uMatrix had a home, I would probably set up a forum, but I do not plan for such thing, I really just want to code, not manage web sites. If you need help to un-break a site when using uMatrix, you can try [Wilders Security](http://www.wilderssecurity.com/threads/umatrix-the-http-switchboard-successor.369601/), where you are likely to receive help if needed, whether by me or other users.

uMatrix can be set to work in [allow-all/block-exceptionally](https://github.com/gorhill/httpswitchboard/wiki/How-to-use-HTTP-Switchboard:-Two-opposing-views#the-allow-allblock-exceptionally-approach) mode with a single click on the `all` cell in the global scope `*`, if you prefer to work this way. This will of course break less sites, but you would then lose all the benefits which comes with block-all/allow-exceptionally mode -- though you will still benefit from the 62,000+ blacklisted hostnames by default.


## License

<a href="https://github.com/gorhill/umatrix/blob/master/LICENSE.txt">GPLv3</a>.
