# Lumen App Store

The package catalogue for [Lumen OS](https://github.com/lumenpearson/os):
programs, fonts, icon sets and bundles, published as static files.

There is no server here. The whole store is JSON — a catalogue, one document
per package, and one payload per version — fetched with `GET` and nothing
else. That is what lets it live in its own repository, deploy on its own, and
be replaced by anyone who wants a different one: Lumen only ever holds a base
URL, in **Settings → Store → Address**.

## The address Lumen uses

```
https://raw.githubusercontent.com/lumenpearson/os-appstore/main/
```

`raw.githubusercontent.com` serves this repository with
`Access-Control-Allow-Origin: *`, so a browser running Lumen on any host can
read it. Files are cached for a few minutes at the edge; a push shows up in
the storefront on the next refresh after that.

## Layout

```
index.json               the catalogue: everything the storefront draws
packages/<id>.json       one package, in full
payload/<id>-<ver>.json  the bytes a download pulls, checked against a digest
banner/<id>.json         a banner's artwork
```

`FORMAT.md` is the contract: every field, what the client refuses, and why.
It is copied from the OS repository, which is where the catalogue is authored.

## Where this comes from

The sources live in the Lumen OS repository under `store/src/`, and
`pnpm store` generates everything here from them — exact byte sizes, real
sha256 digests, and a validation pass that refuses a catalogue with a
duplicate id, a payload that does not parse, or a section naming a package
that does not exist. Editing the generated JSON here by hand will be undone
by the next generation; change the source instead.

## Running your own

Copy this repository, publish it anywhere that serves static files with CORS
open, and point **Settings → Store → Address** at the base URL. Lumen checks
`format`, refuses a catalogue it does not understand whole rather than half
reading it, and verifies every payload's size and digest before installing.
It never sends credentials, and it never tells the store who asked.

## Licence

MIT, the same as Lumen OS. Each font package carries its own licence in the
package document; the two families here are under the SIL Open Font License.
