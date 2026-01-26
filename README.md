# Internet Archive Skill for Claude Code

A Claude Code skill that enables Claude to interact with the [Internet Archive](https://archive.org) using the `ia` command-line tool. Upload, download, search, and manage content on archive.org.

## Features

- **Search** the Internet Archive catalog with advanced query syntax
- **Download** files from any public archive item
- **Upload** files with proper metadata (requires account)
- **Manage metadata** for your uploaded items
- **Automatic tool installation** via `uv` or `pipx` if not present

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed
- Python 3.9 or higher
- One of: `uv`, `pipx`, or `pip` (for installing the `ia` CLI)

## Installation

### Option 1: Plugin Marketplace (Recommended)

Install directly from within Claude Code:

```
/plugin marketplace add https://github.com/internetarchive/internet-archive-skills.git
/plugin install ia@internet-archive-skills
```

### Option 2: Clone and Symlink (For Development)

```bash
# Clone the repository
git clone https://github.com/internetarchive/internet-archive-skills.git
cd internet-archive-skills

# Create skills directory if it doesn't exist
mkdir -p ~/.claude/skills

# Symlink the skill
ln -s "$(pwd)/skills/ia" ~/.claude/skills/ia
```

### Option 3: Direct Copy

```bash
# Clone the repository
git clone https://github.com/internetarchive/internet-archive-skills.git

# Create skills directory and copy
mkdir -p ~/.claude/skills
cp -r internet-archive-skills/skills/ia ~/.claude/skills/
```

### Option 4: Download Just the Skill

```bash
# Create skills directory
mkdir -p ~/.claude/skills/ia

# Download the skill file directly
curl -o ~/.claude/skills/ia/SKILL.md \
  https://raw.githubusercontent.com/internetarchive/internet-archive-skills/main/skills/ia/SKILL.md
```

## Usage

### Manual Invocation

Use the `/ia` command to explicitly invoke the skill:

```
/ia search for public domain books about astronomy
/ia download the apollo 11 mission photos
/ia help me upload my document to archive.org
```

### Automatic Invocation

Claude will automatically use this skill when you mention Internet Archive-related tasks:

```
"Search the Internet Archive for NASA images"
"Download files from archive.org item sci-fi-collection"
"Help me archive this PDF to archive.org"
```

## Examples

### Searching

```
"Search Internet Archive for vintage computer manuals"
"Find public domain audiobooks by Mark Twain on archive.org"
"Search for items in the NASA collection"
```

### Downloading

```
"Download the Gutenberg Bible scans from archive.org"
"Get all PDF files from item 'classic-literature-collection'"
"Download NASA Apollo mission images"
```

### Uploading

```
"Help me upload my thesis to Internet Archive"
"Upload these photos to archive.org with proper metadata"
"Create a new archive item for my podcast episodes"
```

## Configuration

The `ia` CLI tool requires configuration for upload operations. Claude will guide you through this process, but here's a quick overview:

1. **Create an Internet Archive account** at https://archive.org/account/signup
2. **Get your S3-like API keys** at https://archive.org/account/s3.php
3. **Configure the CLI** by running `ia configure` and entering your keys

Configuration is stored in `~/.config/ia.ini`.

Alternatively, set environment variables:
```bash
export IA_ACCESS_KEY_ID="your-access-key"
export IA_SECRET_ACCESS_KEY="your-secret-key"
```

## Plugin Structure

```
internet-archive-skills/
├── .claude-plugin/
│   └── plugin.json       # Plugin manifest
├── skills/
│   └── ia/
│       └── SKILL.md      # Main skill instructions
├── examples/
│   └── usage-examples.md
├── README.md
└── LICENSE
```

The `SKILL.md` file contains:
- Tool detection and installation instructions
- Authentication and configuration guide
- Complete command reference for search, download, upload, and metadata
- Best practices and error handling

## Troubleshooting

### Skill not recognized

1. Ensure the skill is in the correct location: `~/.claude/skills/ia/SKILL.md`
2. Restart Claude Code or reload the conversation
3. Try explicit invocation with `/ia`

### `ia` command not found

Claude will offer to install it for you. If you prefer manual installation:

```bash
# Using uv (recommended)
uv tool install internetarchive

# Using pipx
pipx install internetarchive

# Using pip
pip install internetarchive
```

### Authentication errors

1. Check your credentials at https://archive.org/account/s3.php
2. Re-run `ia configure` to update settings
3. Verify config with `ia configure --print`

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test with Claude Code
5. Submit a pull request

## Resources

- [Internet Archive](https://archive.org)
- [internetarchive Python Library](https://github.com/jjjake/internetarchive)
- [Internet Archive API Documentation](https://archive.org/developers/)
- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)

## License

GNU Affero General Public License v3.0 (AGPL-3.0) - see [LICENSE](LICENSE) file for details.
