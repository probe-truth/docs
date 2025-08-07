# API Reference

Welcome to the ProbeTruth API Reference. This section provides an organized overview of all the available endpoints used to interact with the ProbeTruth deepfake detection system.

---

All endpoints below are prefixed with this base URL.

---

## Authentication

All API requests must include a valid API key in the header:


Refer to the [Authentication Guide](authentication.md) for full details.

---

## Upload Media

Upload a video, audio, or image file for analysis.

**Endpoint**: `POST /v1/media`  
[View full documentation →](upload.md)

---

## Inspect Media

Trigger forensic inspection on previously uploaded media.

**Endpoint**: `POST /v1/inspection`  
[View full documentation →](inspect_media.md)

---

## Generate Forensic Report

Retrieve a detailed report after inspection is completed.

**Endpoint**: `POST /v1/reports`  
[View full documentation →](generate_report.md)

---
