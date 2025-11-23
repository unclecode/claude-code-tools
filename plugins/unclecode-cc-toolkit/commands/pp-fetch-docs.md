# Fetch API Documentation

Fetch and cache API documentation in AI-readable markdown format.

## Arguments

$ARGUMENTS

Library name, URL, or package identifier (e.g., "openai", "stripe", "https://docs.stripe.com")

## Purpose

Download and cache API documentation locally for quick reference across all projects. Docs are stored globally in the plugin directory and available in all chat sessions.

## Storage Location

**Global (available across all projects):**
```
~/.claude/plugins/marketplaces/unclecode-tools/plugins/unclecode-cc-toolkit/api-docs/
```

## Execution Instructions

### Step 1: Parse Input

Extract from query:
- **Library name** (e.g., "openai", "stripe", "nextjs")
- **URL** (if provided, e.g., "https://docs.stripe.com")
- **Version** (optional, e.g., "openai@4.0")

Normalize library name:
- Remove special characters
- Convert to lowercase
- Remove version suffix for directory name

### Step 2: Check if Docs Exist

Check for existing documentation:
```bash
ls ~/.claude/plugins/marketplaces/unclecode-tools/plugins/unclecode-cc-toolkit/api-docs/{library}/
```

**If directory exists:**
- List what's there: `ls -lah api-docs/{library}/`
- Show last modified date
- Ask user:
```
## Documentation Exists

**Library**: {library}
**Location**: api-docs/{library}/
**Last Updated**: {date from stat}
**Files**: {file count} files, {total size}

Documentation already cached. Would you like to:
1. Use existing docs (skip)
2. Update/re-fetch docs
3. View what's cached
```

Wait for user response.

**If doesn't exist:**
- Proceed to Step 3

### Step 3: Fetch Documentation

Try these methods in order:

#### Method 1: Context7 API

**If library name provided (no URL):**

```bash
# Search for library
curl -X GET "https://context7.com/api/v1/search?query={library}" \
  -H "Authorization: Bearer CONTEXT7_API_KEY"

# Get library ID from response
# Then fetch docs
curl -X GET "https://context7.com/api/v1/{library_id}/docs?type=txt&tokens=5000" \
  -H "Authorization: Bearer CONTEXT7_API_KEY"
```

**Note**: Free tier available (no API key needed but rate-limited)

Save response to: `api-docs/{library}/context7-docs.md`

If successful, skip to Step 4.

#### Method 2: llms.txt

**If URL provided OR Context7 doesn't have it:**

1. Extract domain from URL
2. Try to fetch `{domain}/llms.txt`:

```bash
curl -s https://{domain}/llms.txt
```

3. If llms.txt exists:
   - Parse the file (contains list of markdown doc URLs)
   - Fetch each markdown URL listed
   - Save each to: `api-docs/{library}/{filename}.md`

If successful, skip to Step 4.

#### Method 3: Web Scraping

**If neither Context7 nor llms.txt available:**

1. Fetch the docs page HTML
2. Convert HTML to markdown using a clean method:
   - Extract main content (remove nav, footer, ads)
   - Convert to markdown
   - Clean up formatting

3. Save to: `api-docs/{library}/scraped-docs.md`

Add note at top of file:
```markdown
<!-- Scraped from {URL} on {date} -->
<!-- May need manual review for accuracy -->
```

### Step 4: Create Metadata File

Create `api-docs/{library}/metadata.json`:
```json
{
  "library": "library-name",
  "source": "context7|llms.txt|web-scraped",
  "sourceUrl": "original-url",
  "fetchedAt": "2025-01-23T10:30:00Z",
  "fileCount": 3,
  "totalSize": "250KB",
  "version": "4.0.0" (if specified)
}
```

### Step 5: Create Index

If multiple files were fetched, create `api-docs/{library}/INDEX.md`:
```markdown
# {Library} API Documentation

**Fetched from**: {source}
**Date**: {date}
**Version**: {version if known}

## Files

- [context7-docs.md](./context7-docs.md) - Main documentation
- [api-reference.md](./api-reference.md) - API reference
- [guides.md](./guides.md) - Usage guides

## Quick Links

{Parse and list major sections/topics}
```

### Step 6: Confirm Success

Show summary:
```
## Documentation Fetched Successfully

**Library**: {library}
**Source**: {Context7 | llms.txt | Web}
**Location**: ~/.claude/plugins/.../api-docs/{library}/
**Files Saved**: {count} files ({total size})

### What's Available:

{List files with brief description}

### Usage:

These docs are now available globally across all projects.
To reference in your code:
- Ask Claude to "check {library} docs"
- Use /pp-triage for debugging with library context
- Docs automatically available in all future sessions

### Next Steps:

- View docs: cat api-docs/{library}/INDEX.md
- Update later: /pp-fetch-docs {library} (will prompt to update)
```

## Query Examples

```bash
# Fetch by library name (tries Context7 first)
/pp-fetch-docs openai
/pp-fetch-docs stripe
/pp-fetch-docs nextjs

# Fetch by URL (tries llms.txt, then scrapes)
/pp-fetch-docs https://docs.stripe.com
/pp-fetch-docs https://platform.openai.com/docs

# Fetch specific version
/pp-fetch-docs openai@4.0
/pp-fetch-docs react@18

# Update existing docs
/pp-fetch-docs openai
> "Documentation exists. Update? (yes/no)"
> yes
> (re-fetches and overwrites)
```

## Error Handling

**If all methods fail:**
```
## Unable to Fetch Documentation

**Library**: {library}
**Attempted**:
- ❌ Context7: Library not found
- ❌ llms.txt: Not available at {domain}
- ❌ Web scraping: {error reason}

### Suggestions:

1. Check library name spelling
2. Provide full URL to documentation
3. Try alternative library name (e.g., "@openai/sdk" vs "openai")
4. Manually download docs and save to:
   ~/.claude/plugins/.../api-docs/{library}/docs.md

Would you like me to:
- Search for alternative library names?
- Try a different URL?
- Help you manually organize docs?
```

## Notes

- Docs are **global** - stored in plugin directory, not project-specific
- Available across ALL projects and chat sessions
- Context7 preferred (most up-to-date, version-specific)
- llms.txt is official standard (website-published)
- Web scraping is last resort (may need cleanup)
- Update anytime by re-running command
- Large docs may be split into multiple files for better organization

## API Keys (Optional)

For Context7 higher rate limits:
- Get free API key: https://context7.com/dashboard
- Set environment variable: `export CONTEXT7_API_KEY="your-key"`
- Plugin will auto-detect and use it

## Best Practices

**When to fetch:**
- Starting a new project with specific APIs
- Learning a new library
- Need up-to-date reference (re-fetch to update)
- Debugging issues (fresh docs help)

**Storage management:**
- Each library typically: 100-500KB
- Plugin directory has no size limit
- Delete old docs: `rm -rf api-docs/{library}/`
- View all cached: `ls api-docs/`
