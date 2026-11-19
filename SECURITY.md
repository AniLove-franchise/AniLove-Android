# Security policy

## Supported versions

Only the newest release receives fixes. Every release is signed with the same certificate, so an
update installs over the previous version and keeps user data.

## Reporting a vulnerability

**Do not open a public issue for a security problem.**

Use GitHub's private vulnerability reporting: open the **Security** tab of
[AniLove-Android](https://github.com/AniLove-franchise/AniLove-Android) and choose
**Report a vulnerability**. That report is visible only to the maintainer.

Please include:

- the app version (Settings → About) and Android version;
- what an attacker could do, and what they need to start (a link, a malicious file, physical
  access, a rooted device…);
- the shortest reproduction you can manage.

Never paste an AniList token, Google token, email address, private message, or a picture share
link. If a credential may be exposed, revoke it first:
[AniList apps](https://anilist.co/settings/apps) ·
[Google permissions](https://myaccount.google.com/permissions).

## Scope

In scope: the Android APK, this website, the release and update channel, and the picture
encryption scheme.

Out of scope: vulnerabilities in AniList, Google Drive, Tenor, Jikan or the streaming sites the
app can open; requests for features; and anything that requires a rooted device or a modified APK
that no longer matches the published SHA-256.

## What to expect

This is an independent, one-person project, so replies are best effort. A valid report will be
acknowledged as soon as possible, fixed in a new release with a higher `versionCode`, and credited
in the release notes if you want to be named.

There is no bug bounty.
