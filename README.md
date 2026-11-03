# flim-mirror

GitHub Actions (free public-repo runners) → R2 mirror of the public Flim S3 bucket.

- Source: `flim-1-0-2.s3.eu-central-1.amazonaws.com` (public, unsigned GET)
- Dest: Cloudflare R2 bucket `flim-full` (same key layout `full/<hash>.png`)
- Manifests: `manifests/shard-00.txt` … `shard-19.txt` (~69k keys each, all MOVIES)

## Run

```
gh workflow run flim-mirror -f shards=all      # all 20 shards
gh workflow run flim-mirror -f shards=0        # test one shard
```

## Secrets (repo settings)

- `R2_ENDPOINT` = `https://<account_id>.r2.cloudflarestorage.com`
- `R2_KEY`, `R2_SECRET` = R2 API token (Object Read & Write, bucket `flim-full`)

## Notes

- rclone `copy` is resume-friendly (dest size check); rerun freely.
- Runners stream files (14 GB disk), no accumulation.
- Public read of the mirror: enable r2.dev on the bucket (or a custom domain).
- Cost: R2 storage ~$0.015/GB-mo; egress $0. Class A ops from runner uploads.
