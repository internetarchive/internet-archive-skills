# Internet Archive Skill Usage Examples

Real-world examples of using the Internet Archive skill with Claude Code.

> **Note:** For comprehensive command reference and query syntax, see [skills/ia/SKILL.md](../skills/ia/SKILL.md).

## Searching the Archive

### Find Public Domain Books

```
User: Search Internet Archive for public domain science fiction books

Claude: I'll search the Internet Archive for public domain science fiction books.

ia search 'subject:"science fiction" mediatype:texts licenseurl:*publicdomain*' --parameters="rows=20"
```

### Search a Specific Collection

```
User: Find images in the NASA collection

Claude: Let me search the NASA collection for images.

ia search 'collection:nasa mediatype:image' --parameters="rows=25"
```

### Search by Creator

```
User: Find all items uploaded by the Smithsonian

Claude: I'll search for items from the Smithsonian Institution.

ia search 'creator:"Smithsonian"' --parameters="rows=50"
```

### Search with Date Range

```
User: Find audio recordings from the 1920s

Claude: Let me search for audio from that era.

ia search 'mediatype:audio date:[1920-01-01 TO 1929-12-31]' --parameters="rows=30"
```

### Full-Text Search in Text Collections

```
User: Find books that mention "quantum mechanics" in a physics collection

Claude: I'll use full-text search to find that phrase within the text content of books.

ia search -F 'collection:physics_books "quantum mechanics"' --parameters="rows=25"
```

### Full-Text Search for Specific Quote

```
User: I'm looking for documents that contain the phrase "four score and seven years ago"

Claude: Let me search the full text of documents for that exact phrase.

ia search -F '"four score and seven years ago" mediatype:texts'
```

### Full-Text Search in Government Documents

```
User: Search for "climate policy" in US government documents

Claude: I'll perform a full-text search within the government documents collection.

ia search -F 'collection:usgovernmentdocuments "climate policy"' --parameters="rows=50"
```

## Downloading Content

### Download an Entire Item

```
User: Download the "apollo11-photos" item from archive.org

Claude: I'll download all files from that item.

ia download apollo11-photos --destdir=./apollo11-downloads
```

### Download Specific File Types

```
User: I only want the PDF files from item "vintage-computer-manuals"

Claude: I'll download just the PDFs from that collection.

ia download vintage-computer-manuals --glob="*.pdf" --destdir=./manuals
```

### Download with Preview

```
User: What files are in "classic-radio-shows"? I might want to download some.

Claude: Let me first list the files to see what's available.

ia list classic-radio-shows

# Then if you want specific files:
ia download classic-radio-shows --glob="*.mp3" --destdir=./radio-shows
```

### Batch Download from Search

```
User: Download all items from the "prelinger" collection that are movies

Claude: I'll download movies from the Prelinger Archives. This may take a while.

ia download --search 'collection:prelinger mediatype:movies' --glob="*.mp4" --destdir=./prelinger-films
```

## Uploading Content

### Upload a Simple Document

```
User: Help me upload my thesis.pdf to Internet Archive

Claude: I'll help you upload your thesis. First, let me check if the ia tool is configured.

ia configure --print

# If configured, upload with metadata:
ia upload my-thesis-2024 thesis.pdf \
  --metadata="mediatype:texts" \
  --metadata="title:My PhD Thesis" \
  --metadata="creator:Your Name" \
  --metadata="description:Doctoral thesis on topic X" \
  --metadata="subject:computer science;machine learning" \
  --metadata="date:2024"
```

### Upload Multiple Files

```
User: Upload these photos from my vacation folder to archive.org

Claude: I'll help you create an archive item for your photos.

ia upload vacation-photos-2024 ./vacation/*.jpg \
  --metadata="mediatype:image" \
  --metadata="title:Vacation Photos 2024" \
  --metadata="creator:Your Name" \
  --metadata="description:Photos from my summer vacation"
```

### Upload a Video

```
User: I want to archive my documentary film on archive.org

Claude: Let me help you upload your documentary with proper metadata.

ia upload my-documentary-film documentary.mp4 \
  --metadata="mediatype:movies" \
  --metadata="title:My Documentary Film" \
  --metadata="creator:Your Name" \
  --metadata="description:A documentary about..." \
  --metadata="subject:documentary;film" \
  --metadata="date:2024" \
  --metadata="runtime:01:30:00"
```

### Upload with Checksum (Large Files)

```
User: Upload this 5GB archive file, but I want to be able to resume if it fails

Claude: I'll upload with checksum verification so it can resume if interrupted.

ia upload large-dataset-2024 dataset.tar.gz \
  --metadata="mediatype:data" \
  --metadata="title:Research Dataset 2024" \
  --checksum \
  --retries=5
```

## Managing Metadata

### View Item Metadata

```
User: What metadata is on my "research-papers" item?

Claude: Let me fetch the metadata for that item.

ia metadata research-papers
```

### Update Title

```
User: I need to fix the title on my upload

Claude: I'll update the title metadata.

ia metadata my-upload --modify="title:Corrected Title Here"
```

### Add Tags/Subjects

```
User: Add more subject tags to my upload

Claude: I'll append additional subjects to your item.

ia metadata my-upload --append="subject:new-tag"
ia metadata my-upload --append="subject:another-tag"
```

### Fix Creator Name

```
User: The creator name is misspelled on my item

Claude: I'll correct the creator field.

ia metadata my-item --modify="creator:Correct Name Spelling"
```

## Checking Status

### List Files in an Item

```
User: What files are in my uploaded item?

Claude: Let me list all the files.

ia list my-uploaded-item
```

### Check Upload Tasks

```
User: Is my upload still processing?

Claude: I'll check the task status for your item.

ia tasks my-uploaded-item
```

## Workflow Examples

### Complete Research Workflow

```
User: I want to find and download historical weather data, then upload my analysis

Claude: Let me help you with this research workflow.

# 1. Search for weather data
ia search 'subject:"weather data" mediatype:data' --parameters="rows=20"

# 2. Download relevant items
ia download noaa-weather-archive --glob="*.csv" --destdir=./weather-data

# 3. After your analysis, upload results
ia upload weather-analysis-2024 analysis-results.csv figures/*.png \
  --metadata="mediatype:data" \
  --metadata="title:Weather Pattern Analysis 2024" \
  --metadata="creator:Your Name" \
  --metadata="description:Analysis of historical weather patterns"
```

### Backup Project Files

```
User: Help me archive my open source project to archive.org

Claude: I'll help you create an archival copy of your project.

# Create a tarball of your project first
tar -czvf myproject-v1.0.tar.gz ./myproject

# Upload to Internet Archive
ia upload myproject-opensource-v1 myproject-v1.0.tar.gz README.md LICENSE \
  --metadata="mediatype:software" \
  --metadata="title:My Open Source Project v1.0" \
  --metadata="creator:Your Name" \
  --metadata="description:Source code and documentation for MyProject" \
  --metadata="subject:open source;software;programming" \
  --metadata="licenseurl:https://opensource.org/licenses/MIT"
```

## Tips for Common Tasks

### Finding Your Own Uploads

```bash
ia search 'uploader:your@email.com'
```

### Checking if Identifier is Available

```bash
ia metadata desired-identifier-name --exists
```

### Downloading Specific Format

Use `--glob` to match filenames by pattern, or `--format` to download files by their Internet Archive format type:

```bash
# --glob matches filename patterns
ia download nasa --glob='*.jpg'

# --format matches the IA format field (e.g., "Metadata", "JPEG", "MPEG4")
ia download nasa --format=Metadata
```

The difference: `--glob` filters by filename (e.g., `*.mp4`), while `--format` filters by the format field in the item's file metadata (e.g., `h.264`, `Ogg Video`, `Metadata`).

### Excluding Files

Use `--exclude` to skip files matching a glob pattern:

```bash
ia download my-item --exclude="*_thumb*" --exclude="*_spectrogram*"
```
