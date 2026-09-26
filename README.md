# Dikly_releases

## Publishing a new mobile release

Each release adds a new `mobile/Dikly POS {version}.apk`, overwrites the
`mobile/Dikly POS.apk` alias, and updates `mobile/latest.json`.

**Do not delete older versioned APK files.** The installed app auto-checks
`latest.json` and, when a newer version is found, immediately opens the
system browser to start downloading it (see `src/lib/android-update.ts` in
the main repo). That download can sit unfinished or unresumed for a while
on a slow/old device, and Android's download manager can retry the same
URL later. If a release ships in the meantime and the previous version's
APK file has been deleted, that retry 404s — which is exactly what
happened before this note was added (versions 1.0.454 and 1.0.456 were
restored from git history to fix it).

Keep every versioned APK around going forward. They're small relative to
git history that already contains them (deleting the working-tree copy
never reclaimed the space anyway, since the blob was already committed
under the earlier release), so there is no real cost to leaving them in
place, and it removes a whole class of "stale in-flight download" 404s.
