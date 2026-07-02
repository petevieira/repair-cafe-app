# Backups

Your repair data lives in MongoDB. Regular backups protect against accidental deletion and provider issues.

## MongoDB Atlas automated backups

- **M0 (free) tier:** No continuous cloud backup — use manual exports (below)
- **M2+ paid tiers:** Atlas offers continuous backup and point-in-time restore

For production data you cannot afford to lose, consider upgrading to a paid tier or schedule regular exports.

## Manual export with mongodump

Install [MongoDB Database Tools](https://www.mongodb.com/docs/database-tools/installation/installation/) locally.

```bash
mongodump \
  --uri="mongodb+srv://USER:PASSWORD@cluster0.xxxxx.mongodb.net/repaircafe" \
  --out=./backup-$(date +%Y%m%d)
```

This creates a folder of `.bson` and metadata files per collection.

<!-- SCREENSHOT: Example backup folder structure -->

## Restore with mongorestore

```bash
mongorestore \
  --uri="mongodb+srv://USER:PASSWORD@cluster0.xxxxx.mongodb.net" \
  --drop \
  ./backup-20250308/repaircafe
```

`--drop` removes existing collections before restore — use with caution.

## Atlas export (UI)

1. Atlas → **Database** → **Collections**
2. Select a collection → **Export Collection**
3. Choose JSON or CSV for small exports

Best for single-collection recovery; use `mongodump` for full database backups.

## What to back up

| Collection | Contents |
|------------|----------|
| `users` | Login accounts |
| `volunteers` | Volunteer records |
| `repairs` | Repair logs |
| `repairevents` | Events |
| `subscribers` | Newsletter subscribers (if used) |
| `texts` | Terms/waiver text content |

## Backup schedule suggestion

| Cafe size | Suggestion |
|-----------|------------|
| Small, few events/year | Export before and after each event |
| Active | Weekly automated `mongodump` to secure storage |
| Critical | Atlas paid tier + off-site dumps |

Store backups encrypted and restrict access — they contain personal contact information.

## Related pages

- [MongoDB Atlas](../deployment/mongodb-atlas.md)
- [Troubleshooting](./troubleshooting.md)
