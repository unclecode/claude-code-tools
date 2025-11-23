# Project Progress Plugin Version

Show the current version of the unclecode-cc-toolkit plugin.

## Arguments

None

## Purpose

Display plugin version information to verify installation and updates.

## Execution Instructions

### Step 1: Read Plugin Manifest

Read the plugin manifest file:
```bash
cat ~/.claude/plugins/marketplaces/unclecode-tools/plugins/unclecode-cc-toolkit/.claude-plugin/plugin.json
```

### Step 2: Extract Version

Parse JSON and extract the `version` field.

### Step 3: Display Version Info

Output format:
```
## unclecode-cc-toolkit Plugin

**Version**: {version}
**Name**: unclecode-cc-toolkit
**Description**: {description from manifest}

**Repository**: https://github.com/unclecode/claude-code-tools
**Author**: unclecode

---

To update to the latest version:
1. /plugin uninstall unclecode-cc-toolkit
2. /plugin install unclecode-cc-toolkit@unclecode/claude-code-tools
3. Restart Claude Code
```

## Example

```bash
/pp-version
```

Output:
```
## unclecode-cc-toolkit Plugin

**Version**: 1.1.0
**Name**: unclecode-cc-toolkit
**Description**: Comprehensive Claude Code toolkit...

**Repository**: https://github.com/unclecode/claude-code-tools
**Author**: unclecode

---

To update to the latest version:
1. /plugin uninstall unclecode-cc-toolkit
2. /plugin install unclecode-cc-toolkit@unclecode/claude-code-tools
3. Restart Claude Code
```

## Notes

- If plugin.json not found, inform user plugin may not be installed correctly
- Show installation instructions if needed
