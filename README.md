# Tobby Scoop bucket

Scoop manifests for [Tobby](https://github.com/tobby-fetch/tobby-fetch) —
the tool that carries OCI assets (container images, Helm charts, arbitrary
OCI artifacts, packaged file sets) across network zones, down to air-gapped
ones, driven by signed Recipes.

## Install

```powershell
scoop bucket add tobby https://github.com/tobby-fetch/scoop-bucket
scoop install tobby
```

Then:

```powershell
tobby version
```

## Why Scoop

Windows is where the mirror operator actually sits. The complete mirror
workflow — manual synchronization, the self-contained store on removable
media, file-based logging on the medium, OCI image layout export and
import, and destination-side operation — is inside Tobby's validated
Windows scope, exercised on a Windows runner on every pull request.

Scoop installs **per user, with no administrator rights**, which is
frequently the only kind of install available on a managed workstation —
and a managed workstation is exactly what sits at the edge of a controlled
zone.

## What you get

The manifest installs the official Windows release binaries (x64 and
arm64) — the same reproducible artifacts as every other release target,
each with a CycloneDX SBOM and SLSA build provenance attached to the
[GitHub release](https://github.com/tobby-fetch/tobby-fetch/releases).

Scoop verifies the pinned SHA-256 on download. Those checksums are taken
from the release's own `SHA256SUMS` — the manifest the SLSA provenance
covers — and never recomputed from a re-download, which would only attest
to whatever the network returned.

The release asset is architecture-qualified so that six binaries can share
one release page; the `#/tobby.exe` URL fragment is Scoop's
rename-on-download, so the command an operator types stays `tobby`.

## License

The manifests in this bucket are published under
[GPL-3.0-only](https://www.gnu.org/licenses/gpl-3.0.html), like Tobby
itself. © 2026 infraBuilder SASU and contributors.

## How this bucket stays current

`bucket/tobby.json` carries `checkver` and `autoupdate`, so
[`scoop-update`](.github/workflows/scoop-update.yml) can follow new
releases from here without a push from the main repository: it reads the
latest release's `SHA256SUMS` and rewrites the two hashes from it, keeping
the same provenance-covered checksums a manual bump would use.

The release workflow of
[tobby-fetch](https://github.com/tobby-fetch/tobby-fetch) renders the
manifest on every tag and attaches it to the release, but deliberately
pushes it nowhere: a bucket is an index other people install from, and
putting a version in front of them stays a reviewed act rather than a side
effect of tagging.
