# fmcb-assets

Public mirror for large build artefacts used by the FMCB project.
Anything that benefits from a stable HTTPS download URL (so an FMCB box
can `curl` it without git or LFS installed) belongs here. Source code,
docs, infra, and tests live in the private repos.

`.jar` files are tracked via Git LFS (`.gitattributes`).

## `fineract-provider-1.14.0-m4-pdfname.jar`

Apache Fineract 1.14.0 rebuilt from upstream `apache/fineract@1.14.0`
with one production patch:

- **`ReadReportingServiceImpl.retrieveReportPDF`** — report-name
  validation regex relaxed from `^[a-zA-Z0-9_.-]+$` to
  `^[a-zA-Z0-9_.\\- ]+$`, and the on-disk PDF filename collapses
  whitespace to underscores. Lets reports with spaces in their names
  (every CUA / BoG report we use) export to PDF. Path-traversal check
  on the normalised `Path` still in place.

Deploy as `fineract-provider-1.14.0.jar` on FE1 + FE2 per the same
recipe documented for prior assets — back up the existing JAR
timestamped, install with owner `appusr:appusr` and mode `0644`,
SIGKILL the running JVM by PID (not by `pkill -f` which catches the
relay agent's own command line), let systemd restart.
