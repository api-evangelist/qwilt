---
name: Attach a TLS certificate to a Qwilt CDN site
description: >-
  Create or upload a TLS certificate in the Qwilt Certificate Manager so a CDN
  site can serve HTTPS, optionally via a Qwilt-managed CSR template workflow.
api: openapi/qwilt-certificate-manager-openapi.yml
operations: [listCertificateTemplates, createCertificateTemplate, createCertificate, getCertificate]
---

# Attach a TLS certificate to a Qwilt CDN site

Use the Qwilt Certificate Manager API (`https://cert-manager.cqloud.com`) to
manage the TLS material that secures a CDN site.

## Authenticate
Send `Authorization: X-API-KEY <api-key>` (Editor permission level).

## Steps
1. **(Optional) Prepare a template** — for a Qwilt-managed CSR workflow, list
   templates with `GET /api/v2/certificate-templates` (`listCertificateTemplates`)
   or create one with `POST /api/v2/certificate-templates`
   (`createCertificateTemplate`).
2. **Create the certificate** — `POST /api/v2/certificates` (`createCertificate`),
   either uploading your own certificate + private key (self-managed) or
   referencing a template for Qwilt-managed lifecycle. Capture the returned `certId`.
3. **Confirm** — `GET /api/v2/certificates/{certId}` (`getCertificate`) to verify
   status before linking it to a site.

## Rules
- On `401` re-check the API key; on `403` the key lacks Editor permission.
- Qwilt-managed certificate lifecycle automates renewal via the CSR template;
  self-managed certificates must be re-uploaded before expiry.
