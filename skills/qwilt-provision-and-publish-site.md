---
name: Provision and publish a Qwilt CDN site
description: >-
  Create a Qwilt CDN media-delivery site, add a versioned configuration (hosts,
  origins, rules), and publish it live on the Qwilt Open Edge Cloud.
api: openapi/qwilt-media-delivery-openapi.yml
operations: [createSite, createSiteConfiguration, createPublishingOperation, getPublishingOperation]
---

# Provision and publish a Qwilt CDN site

Use the Qwilt CDN Sites API (`https://media-sites.cqloud.com`) to stand up a new
media-delivery site and take it live.

## Authenticate
Send `Authorization: X-API-KEY <api-key>` on every request. Create the key in QC
Services under Administration > API Keys with an **Editor** permission level
(Viewer keys are read-only). Keys need up to five minutes to propagate.

## Steps
1. **Create the site** — `POST /api/v2/sites` (`createSite`) with the site body.
   Capture the returned `siteId`.
2. **Add a configuration revision** — `POST /api/1/sites/{siteId}/configurations`
   (`createSiteConfiguration`) with the `hosts` array (host, host-metadata,
   origin endpoints, rules, paths). Capture the returned `revisionId`.
3. **Publish** — `POST /api/v2/sites/{siteId}/publishing-operations`
   (`createPublishingOperation`) referencing the `revisionId`. Capture `publishId`.
4. **Poll status** — `GET /api/v2/sites/{siteId}/publishing-operations/{publishId}`
   (`getPublishingOperation`) until the operation completes.

## Rules
- There is no documented idempotency-key header; do not blindly retry POSTs.
  Re-check with the list/get endpoints before recreating an object.
- On `401` re-check the API key; on `403` the key is a Viewer (needs Editor).
- Publishing is an explicit action — use `un-publish` / `republish` /
  `cancel` action endpoints rather than mutating a live config in place.
