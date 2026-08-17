# 00 Inbox

Landing zone for unprocessed discoveries.

## Structure

```text
00-inbox/
├── clippings/
├── links/
└── unsorted/
```

For local vault imports use:

```text
00-inbox/clippings/<batch-id>/
```

Each large batch should include `MANIFEST.md` and later `PROCESSING_REPORT.md`.

Raw files may preserve original names. Canonical curated records must not.

See `../INGESTION_GUIDE.md` before uploading a batch.
