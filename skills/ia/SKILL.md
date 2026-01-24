---
name: ia
description: Interact with Internet Archive (archive.org) - upload files, download items, and search the archive using the ia CLI tool. Use when working with archive.org, archiving content, or retrieving historical data.
allowed-tools: Bash
---

# Internet Archive CLI Skill

This skill enables interaction with the Internet Archive (archive.org) using the `ia` command-line tool from the `internetarchive` Python package.

## Tool Detection and Installation

Before using any `ia` commands, check if the tool is installed:

```bash
ia --version
```

If the `ia` command is not found, install it using `uv`:

```bash
uv tool install internetarchive
```

Alternative installation methods:
- `pipx install internetarchive`
- `pip install internetarchive`

After installation, verify it works with `ia --version`.

## Configuration and Authentication

Check if `ia` is configured:

```bash
ia configure --print
```

If not configured (shows error or empty), the user needs to set up credentials:

1. **Interactive setup**: Run `ia configure` and follow prompts
2. **Get credentials**: IA-S3 keys from https://archive.org/account/s3.php
3. **Config location**: Saves to `~/.config/ia.ini`

Environment variable alternative:
```bash
export IA_ACCESS_KEY_ID="your-access-key"
export IA_SECRET_ACCESS_KEY="your-secret-key"
```

Note: Configuration is required for uploads and metadata modifications. Searching and downloading public items works without authentication.

## Search Operations

Search the Internet Archive catalog:

```bash
ia search '<query>'
```

### Search Parameters

- Pagination: `--parameters="page=N&rows=N"` (default rows=50)
- Output format: `--itemlist` (identifiers only, one per line)
- Sort: `--parameters="sort[]=field+asc"` or `sort[]=field+desc`

### Search Query Syntax

| Query | Description |
|-------|-------------|
| `'subject:"topic"'` | Search by subject |
| `'collection:name'` | Items in a collection |
| `'creator:"Author Name"'` | By creator/author |
| `'title:"search text"'` | By title |
| `'mediatype:texts'` | By media type |
| `'date:[2020-01-01 TO 2024-12-31]'` | Date range |

Combine queries with AND, OR, NOT:
```bash
ia search 'collection:nasa AND mediatype:image'
```

### Examples

```bash
# Search NASA images
ia search 'collection:nasa mediatype:image' --parameters="rows=10"

# Search public domain books
ia search 'subject:"public domain" mediatype:texts'

# Get just identifiers
ia search 'creator:"Mark Twain"' --itemlist
```

## Download Operations

Download files from an Internet Archive item:

```bash
ia download <identifier>
```

### Download Parameters

| Parameter | Description |
|-----------|-------------|
| `--glob="*.ext"` | Download only matching files |
| `--exclude="*pattern*"` | Exclude files matching pattern |
| `--destdir=path` | Download to specific directory |
| `--no-directories` | Flatten directory structure |
| `--dry-run` | Show what would be downloaded |
| `--checksum` | Skip files that already exist with correct checksum |

### Examples

```bash
# Download all files from an item
ia download nasa_apollo_images

# Download only JPG files
ia download nasa_apollo_images --glob="*.jpg"

# Download to specific directory
ia download nasa_apollo_images --destdir=./downloads

# Download from search results
ia download --search 'collection:opensource_movies' --glob="*.mp4"

# Preview what will be downloaded
ia download my_item --dry-run
```

## Upload Operations

Upload files to the Internet Archive (requires authentication):

```bash
ia upload <identifier> file1 file2 --metadata="mediatype:value"
```

### Required Metadata

The `mediatype` field is required. Common values:
- `texts` - Books, documents, PDFs
- `movies` - Video files
- `audio` - Music, podcasts, sound
- `software` - Programs, games
- `image` - Photos, graphics
- `data` - Datasets, archives

### Upload Parameters

| Parameter | Description |
|-----------|-------------|
| `--metadata="key:value"` | Set metadata (repeatable) |
| `--header="key:value"` | Set HTTP header |
| `--checksum` | Skip files already uploaded |
| `--no-derive` | Skip derivative processing |
| `--retries=N` | Number of retry attempts |

### Common Metadata Fields

```bash
--metadata="title:My Document Title"
--metadata="creator:Author Name"
--metadata="description:A description of the content"
--metadata="subject:topic1;topic2"
--metadata="collection:community_texts"
--metadata="date:2024-01-15"
--metadata="language:eng"
```

### Examples

```bash
# Upload a PDF document
ia upload my-document-2024 document.pdf \
  --metadata="mediatype:texts" \
  --metadata="title:My Document" \
  --metadata="creator:John Doe"

# Upload multiple files
ia upload my-archive file1.pdf file2.pdf file3.pdf \
  --metadata="mediatype:texts" \
  --metadata="title:Document Collection"

# Upload with checksum verification
ia upload my-item large-file.zip \
  --metadata="mediatype:data" \
  --checksum

# Bulk upload using spreadsheet
ia upload --spreadsheet=metadata.csv
```

### Identifier Guidelines

- Use lowercase letters, numbers, and hyphens
- No spaces or special characters
- Keep it descriptive but concise
- Check if identifier exists: `ia metadata <identifier>`

## Metadata Operations

View and modify item metadata:

```bash
# View metadata
ia metadata <identifier>

# Modify metadata
ia metadata <identifier> --modify="field:value"

# Append to existing field
ia metadata <identifier> --append="subject:new-topic"

# Remove metadata field
ia metadata <identifier> --remove="field"

# Bulk updates from spreadsheet
ia metadata --spreadsheet=metadata.csv
```

## List Operations

List files in an Internet Archive item:

```bash
ia list <identifier>
```

Shows all files with details (name, size, format).

Parameters:
- `--columns=name,size` - Specify columns to show
- `--glob="*.pdf"` - Filter by pattern

## Tasks and Jobs

Check status of uploads and other operations:

```bash
ia tasks <identifier>
```

## Best Practices

1. **Always configure before uploading** - Run `ia configure` first
2. **Use meaningful identifiers** - Descriptive, lowercase, hyphenated
3. **Include proper metadata** - At minimum: mediatype, title, creator
4. **Check before uploading** - Verify identifier doesn't exist: `ia metadata <id>`
5. **Use checksums** - Add `--checksum` for large uploads to enable resume
6. **Respect rate limits** - Don't spam requests; add delays for bulk operations
7. **Test with dry-run** - Use `--dry-run` to preview operations

## Error Handling

| Error | Solution |
|-------|----------|
| "not configured" | Run `ia configure` or set environment variables |
| "identifier exists" | Choose a different identifier |
| "permission denied" | Check credentials at https://archive.org/account/s3.php |
| "network error" | Retry the operation; check internet connection |
| "item not found" | Verify the identifier spelling |

## Quick Reference

```bash
# Search
ia search 'query'

# Download
ia download <identifier>
ia download <identifier> --glob="*.pdf"

# Upload (requires auth)
ia upload <identifier> files --metadata="mediatype:texts"

# Metadata
ia metadata <identifier>
ia metadata <identifier> --modify="title:New Title"

# List files
ia list <identifier>

# Check config
ia configure --print

# Install
uv tool install internetarchive
```
