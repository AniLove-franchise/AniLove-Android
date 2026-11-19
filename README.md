# AniLove for Android

**An AniList client with an Instagram-style inbox that runs on AniList messages.**

[**⬇ Download the latest APK**](https://github.com/AniLove-franchise/AniLove-Android/releases/latest/download/AniLove.apk) · [All releases](https://github.com/AniLove-franchise/AniLove-Android/releases) · [Privacy policy](https://anilove-franchise.github.io/AniLove-Android/privacy.html)

Android 7.0+ · about 13 MB · free · no ads · no analytics · no AniLove server

---

## What this repository is

This repository hosts **release builds only**: the signed APK, its checksum and release notes.
There is no source code here.

- **Download** from [Releases](https://github.com/AniLove-franchise/AniLove-Android/releases).
- **Report a problem** in [Issues](https://github.com/AniLove-franchise/AniLove-Android/issues).
- **Read about your data** in the [privacy policy](https://anilove-franchise.github.io/AniLove-Android/privacy.html).

Every release is built from the same codebase and signed with the same certificate, so a new
version installs straight over the old one and keeps your data.

## What it does

AniLove is a full AniList client, not a companion app:

- **Lists and tracking** — anime and manga lists, scores, progress, status, favourites, activity,
  seasonal charts, airing schedules and search.
- **Messaging** — an inbox with conversations, replies, reactions, edited messages, seen marks and
  deletion. Messages are stored on AniList as ordinary AniList messages, so they keep working in
  a browser too.
- **Pictures in chat** — photos and GIFs are encrypted on the phone and sent through your own
  Google Drive. The person receiving them needs no Google account and no extra app beyond
  AniLove.
- **GIFs and stickers** — Tenor search, one tap to send.
- **Episode streaming** — play from the sources you choose, in a native player with subtitles,
  gestures, picture-in-picture and offline downloads.
- **Discover** — character art, wallpapers, quotes and anime news from public providers.
- **Several accounts** — keep more than one AniList profile on the device, separated behind an
  optional PIN, each with its own token.

There is no AniLove account, no sign-up and no developer-operated backend. The app talks
directly to AniList and, only when you use the matching feature, to the public services listed
further down.

## Requirements

| | |
| --- | --- |
| Android | 7.0 (API 24) or newer |
| Storage | about 40 MB installed; episodes you download use more |
| Account | an [AniList](https://anilist.co) account — the only way to sign in |
| Google account | optional, and only on the **sender's** side if you want to send pictures |
| Play Store | not required, and not used |

## Installing

1. **Download `AniLove.apk`** from the [latest release](https://github.com/AniLove-franchise/AniLove-Android/releases/latest/download/AniLove.apk).
2. **Open the file.** Android will ask permission to install from your browser or file manager.
   Approve it once, then tap **Install**. If Play Protect shows a warning about an unknown
   developer, that is expected for sideloaded apps — see [verifying the download](#verifying-the-download).
3. **Open AniLove and sign in with AniList.** Sign-in happens on AniList's own page; the app
   never sees your password.
4. **Optional:** connect Google Drive from the chat composer if you want to send pictures.

### One-time note for anyone who used a build before v1.3

Earlier test builds were signed with a different key. Android will refuse to update across that
change, so uninstall the old app first. That also clears the locally saved AniList session, so
sign in once again. Every release from v1.3 onward uses the permanent key and updates normally.

## Updates

- New versions install over the old one and keep your lists, chats, settings and sessions.
- A future version will be able to check for updates in-app. For now, watch **Releases** or use
  the "Watch → Custom → Releases" button on this repository.
- Android itself verifies the signing certificate on every install. An APK signed by anyone else
  is rejected, even if it has the same package name.

## Signing in

AniLove uses AniList's OAuth page and one scope: your AniList account. There is no password
field inside the app.

- Your token is stored in app-private storage, separated per profile.
- Android cloud backup is disabled for the app, so the token is not copied off the device.
- You can revoke access at any time at [anilist.co/settings/apps](https://anilist.co/settings/apps).

## Encrypted pictures — how it works, and its honest limit

AniList messages are text-only, so a picture is sent as a link:

```
https://drive.google.com/file/d/<file-id>#anilove=<key>
```

1. The photo is re-encoded on the phone at up to 1600 px, which also strips EXIF and GPS.
2. It is encrypted with **AES-256-GCM** using a fresh random key for that file.
3. The encrypted bytes are uploaded to **your** Google Drive with the narrow `drive.file`
   permission — access only to files the app itself creates.
4. The file is marked "anyone with the link can view", so the receiver needs no Google account.
5. The key travels in the URL fragment (after `#`), which browsers never send to a server.

**What this protects:** Google, link scanners and anyone who finds the link only ever gets
unreadable bytes.

**What it does not protect:** AniList and the person you are talking to. The key is inside the
chat message, and AniList stores that message. This is encryption at rest and in transit, not
end-to-end encryption against the chat service. AniLove says so plainly instead of pretending
otherwise.

Sending pictures is entirely optional and works only for the sender; the receiver just needs
AniLove to decrypt.

## Permissions

The APK requests five Android permissions plus one app-private signature permission:

| Permission | Why |
| --- | --- |
| `INTERNET` | Talk to AniList and the services below |
| `ACCESS_NETWORK_STATE` | Know whether a connection exists before retrying |
| `FOREGROUND_SERVICE` | Keep an episode download or upload alive when you leave the app |
| `FOREGROUND_SERVICE_DATA_SYNC` | Declare that foreground work as data transfer (Android 14+) |
| `POST_NOTIFICATIONS` | Show download progress and new-message alerts (Android 13+; you can decline) |
| `com.anilove.app.DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION` | Internal AndroidX permission so only this app can send it its own broadcasts |

Not requested: contacts, calendars, SMS, call logs, location, camera, microphone, body sensors,
the photo library, or "install unknown apps" (the app cannot install anything by itself).
Pictures are picked through Android's system photo picker, which hands the app only the single
item you choose.

## What the app connects to

No traffic goes through a developer server. These are the real destinations:

| Destination | When | What leaves the device |
| --- | --- | --- |
| `graphql.anilist.co`, `anilist.co`, `s4.anilist.co` | Always, once signed in | Your API requests and token; your IP |
| `drive.google.com`, `drive.usercontent.google.com`, `lh3.googleusercontent.com` | Only if you connect Drive | The encrypted file, or a download request |
| `tenor.com` | You open the GIF picker | Your search words |
| `api.jikan.moe` | Anime/manga details, news, schedules | The title or ID being viewed |
| `api.ani.zip`, `api.trace.moe` | Episode metadata; optional scene search | Show/season, or the frame you search |
| Streaming providers you select | You press play | Ordinary web requests; those sites see your IP and may set cookies in the player |
| Image, wallpaper and quote APIs | You open those screens | Ordinary image requests; no account data |

All traffic is HTTPS. Cleartext HTTP is blocked at the platform level, and nothing is proxied
through the developer.

## Privacy in one paragraph

The developer operates no server, no database, no analytics and no crash reporting, and receives
no AniList token, Google token, message, picture or file list. Your AniList account is handled by
AniList; your pictures are handled by your own Google Drive, encrypted first. Everything else
stays on the phone. Full details, including every category of local data and every third party,
are in the **[privacy policy](https://anilove-franchise.github.io/AniLove-Android/privacy.html)**.

## Verifying the download

Every release lists the APK's SHA-256 in its notes. On Windows:

```powershell
certutil -hashfile AniLove.apk SHA256
```

On macOS or Linux:

```sh
shasum -a 256 AniLove.apk
```

The result must match the release notes exactly. To check who signed it (Android SDK build-tools):

```sh
apksigner verify --print-certs AniLove.apk
```

The certificate SHA-256 must always be:

```
E3:04:A1:D5:2A:6D:27:29:FF:A0:4B:2F:6A:F7:7B:84:3E:CA:C9:99:10:23:A6:E1:99:AE:57:9C:CD:48:00:24
```

If either value differs, do not install the file.

## FAQ

**Is AniLove on the Play Store?**
No. It is distributed here as a signed APK, on purpose: no store account, no store services, no
store review, and updates that do not depend on a store listing.

**Does it need root, Xposed or a patched Android?**
No. A normal Android 7.0+ phone is enough.

**Why does Android warn me about this file?**
Android warns about any app that did not come from a store. Verify the checksum and certificate
above if you want certainty.

**Do I need Google Drive to use the app?**
No. Everything except sending pictures works without a Google account. Receiving pictures also
needs no Google account.

**Can the developer read my messages or pictures?**
No. Messages live on AniList; pictures are encrypted before upload and the key never goes to a
developer server, because there is no developer server. AniList itself can read message text and
the picture key — that limitation is described above.

**Does it track me?**
There is no analytics, telemetry, advertising ID or crash reporter in the app.

**Can I use more than one AniList account?**
Yes. Profiles are kept separately on the device, each optionally behind a PIN.

**What happens if I uninstall it?**
Local settings, cached chats and the saved AniList session are removed. Your AniList lists and
messages stay on AniList; pictures stay in your Drive until you delete them.

**Why is the APK not tiny?**
It bundles a native video player (AndroidX Media3), the WebView app shell and Google Sign-In.
About 13 MB is the honest cost of those features.

**Is this affiliated with AniList, Google, Tenor or MyAnimeList?**
No. AniLove is an unofficial, independent client.

## Reporting a problem

Open an [issue](https://github.com/AniLove-franchise/AniLove-Android/issues) and include:

- app version (Settings → About) and Android version;
- what you expected and what happened;
- steps that reproduce it, if you know them.

For anything security-related, please describe the problem without posting tokens, email
addresses, private messages or picture links. Revoke the affected access first if a credential
may have leaked.

## Disclaimer

AniLove is an unofficial client. It is not affiliated with, endorsed by or sponsored by AniList,
Google, Tenor or MyAnimeList. Streaming sources are third-party websites reached from the app;
AniLove hosts no video and no media. Use the app and those sources in accordance with the laws
and terms that apply to you.
