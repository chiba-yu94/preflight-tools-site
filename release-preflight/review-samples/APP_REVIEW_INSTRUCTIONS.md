# Release Preflight — App Review samples

Product: **Release Preflight 1.0.0 (3)**, native macOS 14 or later.
These original, synthetic fixtures are intended for inspection, not execution.
No login, issued API key, private key, commercial library or backend is required.
The app under review has not been changed for this sample delivery.

## Permanent downloads

The fixed `release-preflight-review-samples-v1` release and these assets will be
retained for future reviews. Future revisions use new tags; these files will not
be replaced during review.

- [Clean Xcode Archive ZIP](https://github.com/chiba-yu94/preflight-tools-site/releases/download/release-preflight-review-samples-v1/ReleasePreflight-Clean-Sample.zip)
- [Findings Xcode Archive ZIP](https://github.com/chiba-yu94/preflight-tools-site/releases/download/release-preflight-review-samples-v1/ReleasePreflight-Findings-Sample.zip)
- [Source project / folder / ZIP sample](https://github.com/chiba-yu94/preflight-tools-site/releases/download/release-preflight-review-samples-v1/ReleasePreflight-Source-Sample.zip)
- [HTML and JSON report examples](https://github.com/chiba-yu94/preflight-tools-site/releases/download/release-preflight-review-samples-v1/ReleasePreflight-Report-Examples.zip)
- [SHA-256 checksums](https://github.com/chiba-yu94/preflight-tools-site/releases/download/release-preflight-review-samples-v1/SHA256SUMS.txt)
- [Public instructions and download page](https://chiba-yu94.github.io/preflight-tools-site/release-preflight/review-samples/)

## Open an Xcode Archive

1. Download either Archive ZIP above. In Finder, double-click the **ZIP** to expand
   it. This restores `ReleasePreflight-Clean-Sample.xcarchive` or
   `ReleasePreflight-Findings-Sample.xcarchive`. There is no password.
2. Open **Release Preflight** (the app being reviewed), leaving **Check known
   dependency vulnerabilities** OFF for the initial local inspection.
3. Click **Choose File** (Japanese: **ファイルを選択**) or press Command-O. Select
   the extracted **.xcarchive package itself**, then click **Open**. The same
   panel accepts folders, ZIPs, apps and Xcode archives.
4. Inspection starts automatically; there is no separate Run button. Wait for
   the score, counts and findings. The original files are not modified.
5. Choose **All categories** or a specific category, then select a finding to read
   **What we found**, **Why it matters** and **What to do**. Potential secret
   matches are masked.
6. Click **Export HTML** / **Export JSON** (Japanese: **HTMLを書き出す** /
   **JSONを書き出す**) and choose a save destination. Open the HTML in a browser
   or JSON in a text viewer to inspect the report. Reports use English text.

Do not double-click the **.xcarchive** in Finder to begin the scan: macOS may open
Xcode Organizer instead. You do not need to run the sample executable or allow
it through Gatekeeper. Choose it inside Release Preflight.

## Expected results in build 3

| Extracted archive, lookup OFF | Files inspected | Blockers | Warnings | Score |
|---|---:|---:|---:|---:|
| Clean | 6 | 0 | 0 | 100 |
| Findings | 10 | 1 | 6 | 50 |

These are observed results for the fixed v1 fixtures before any ignores.

The Clean archive is a real universal arm64/x86_64 Xcode archive with archive
Info.plist, a macOS app, bundle metadata, an executable and a privacy manifest.
It can still show informational **Privacy manifest needs review**: build 3 checks
an archive-level standard resource location while an Xcode archive stores its
app's manifest under `Products/Applications/...app/Contents/Resources`. The
message itself notes that archives may use a different location. This INFO does
not mean archive parsing failed, and does not reduce the score.

The Findings archive deliberately contains:

- **Unrestricted network loads allowed**: `NSAllowsArbitraryLoads` in app Info.plist.
- **Debugger attachment allowed**: `get-task-allow` in the app's ad-hoc signed
  entitlements; no personal signing identity or provisioning profile is included.
- **Potential secret assignment**: an invented, nonfunctional value, masked as
  `not_••••••••2345` in the UI and reports.
- **Debug mode enabled**, **Development environment selected**, **TLS verification
  disabled**: inert text settings, never executed.
- **Development file needs review**: an original synthetic `debug.log`.

These settings belong only to the sample, not to Release Preflight's runtime.

## Other implemented features

- **Ignore this finding** excludes only that finding from the current score.
  **Ignore this rule for this scan** excludes matching rule findings for this scan.
  The source is unchanged. Reopen/rescan the sample to reset ignores.
- **App input**: from the extracted archive, choose the contained
  `Products/Applications/Preflight Review Sample.app` in the same Open panel.
  The app need not be launched. Clean app: 5 files; Findings app: 9 files.
- **Folder input**: expand the Source Sample ZIP and select the resulting
  `ReleasePreflight-Source-Sample` folder. It contains original sample SwiftUI
  source and synthetic configuration/dependency files.
- **ZIP input**: select any downloaded sample ZIP directly without expansion.
  Its files are inspected in memory, without extraction. For **signed entitlement
  metadata**, use an extracted archive/app: ZIP mode does not inspect the signing
  metadata of a nested app as a macOS code object. Finding counts may differ.
- **Drag & Drop**: drag a selected folder, ZIP, app or archive from Finder onto
  Release Preflight. This is an alternative to **Choose File**.
- **Optional online dependencies**: the Findings archive and Source Sample include
  `package-lock.json` pinning public package `lodash` to `4.17.20`. Enable
  **Check known dependency vulnerabilities**, read the disclosure (including the
  private-package-name risk), then explicitly choose **Enable lookup**. Select
  the same sample again. The lookup requires internet access to `api.osv.dev`.
  OSV returned five advisory IDs in validation on November 1, 2026; current IDs
  and counts depend on OSV. When the service is unavailable, local results remain
  available and dependency lookup is marked unavailable. OFF requires no network.

## Troubleshooting

- Expand the ZIP before selecting the archive when testing signed entitlements.
- Select the `.xcarchive` package rather than only its archive Info.plist.
- If a category hides findings, return to **All categories**.
- To recover the baseline counts, rescan with lookup OFF and no ignores.
- Export through the native Save panel to a writable user-selected destination.
- Review Scan Coverage messages; the score is not a security certification.

## Provenance and validation

Generated with Xcode 27.0 using its Archive action, followed by original fixture
resource insertion and ad-hoc signing (no certificate/team/private key). Both ZIPs
were integrity-checked, extracted and structurally validated. The submitted
TestFlight installation of Release Preflight 1.0.0 build 3 on a physical Mac was
used to open both archives, inspect findings and save HTML/JSON reports. Automated
checks also exercise the same core's archive, ZIP and app scans, synthetic-value
masking, and optional OSV lookup. Public downloads are separately hash-checked
and extracted before delivery to App Review. See SHA256SUMS for the fixed assets.
