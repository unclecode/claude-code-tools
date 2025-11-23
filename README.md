# Claude Code Tools

A comprehensive toolkit for [Claude Code](https://claude.ai/claude-code) by unclecode. All-in-one productivity suite with project management, image generation, and testing capabilities.

## The Plugin: unclecode-cc-toolkit

One plugin with everything you need for productive development workflows.

### What's Included

**Slash Commands (9 commands):**
- `/pp-init` - Initialize project progress tracking
- `/pp-add` - Add a checkpoint to project progress
- `/pp-checkpoint` - Create a project checkpoint
- `/pp-status` - Show current project progress status
- `/pp-resume` - Resume from a previous checkpoint
- `/pp-update` - Update project progress information
- `/pp-clean` - Clean up project progress data
- `/pp-migrate` - Migrate project progress to new format
- `/pp-remove` - Remove a checkpoint from project progress

**Skills (2 skills):**

1. **gemini-imagegen**
   - Generate images from text prompts
   - Edit existing images
   - Create logos, mockups, stickers
   - Multi-turn refinement
   - Requires `GEMINI_API_KEY`

2. **webapp-testing**
   - Automated browser testing with Playwright
   - Server lifecycle management
   - Network idle detection
   - Screenshot verification
   - Perfect for local development testing

## Installation

### Add the Marketplace

```bash
/plugin marketplace add unclecode/claude-code-tools
```

### Install the Toolkit

```bash
/plugin install unclecode-cc-toolkit@unclecode/claude-code-tools
```

After installation, restart Claude Code to activate.

## Usage

All commands and skills are immediately available after installation:

```bash
# Use project progress commands
/pp-init

# Skills are invoked automatically by Claude when needed
"Generate a logo for my startup"
"Test my Next.js app at localhost:3000"
```

## Repository Structure

```
claude-code-tools/
├── .claude-plugin/
│   └── marketplace.json
└── plugins/
    └── unclecode-cc-toolkit/
        ├── .claude-plugin/
        │   └── plugin.json
        ├── commands/          # All PP commands
        └── skills/            # gemini-imagegen, webapp-testing
```

## Contributing

Contributions are welcome! Feel free to submit issues or pull requests.

## License

MIT License - see LICENSE file for details

## Author

**unclecode**
- GitHub: [@unclecode](https://github.com/unclecode)

## Support

For issues, questions, or feature requests, please open an issue on GitHub.
