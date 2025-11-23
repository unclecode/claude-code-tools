# Claude Code Tools

A collection of productivity tools and plugins for [Claude Code](https://claude.ai/claude-code) by unclecode. This marketplace provides workflow automation, project management utilities, and development enhancers.

## Available Plugins

### Project Progress

Project progress tracking and checkpoint management system for Claude Code.

**Features:**
- Track project milestones and evolution
- Create and manage checkpoints
- Resume from previous states
- AI-optimized progress documentation
- Clean and migrate project data

**Commands:**
- `/pp-init` - Initialize project progress tracking
- `/pp-add` - Add a checkpoint to project progress
- `/pp-checkpoint` - Create a project checkpoint
- `/pp-status` - Show current project progress status
- `/pp-resume` - Resume from a previous checkpoint
- `/pp-update` - Update project progress information
- `/pp-clean` - Clean up project progress data
- `/pp-migrate` - Migrate project progress to new format
- `/pp-remove` - Remove a checkpoint from project progress

## Installation

### Add the Marketplace

```bash
/plugin add-marketplace unclecode/claude-code-tools@github
```

### Install Plugins

```bash
# Install project progress tracking
/plugin install project-progress@unclecode/claude-code-tools
```

After installation, restart Claude Code to activate the plugins.

## Usage

Once installed, all commands are available via the slash command interface. Use `/help` to see all available commands.

### Example Workflow

```bash
# Initialize project tracking
/pp-init

# Create a checkpoint
/pp-checkpoint

# Check progress
/pp-status

# Resume from a checkpoint
/pp-resume
```

## Development

This repository uses a monorepo structure with multiple plugins:

```
claude-code-tools/
├── marketplace.json          # Marketplace configuration
└── plugins/
    └── project-progress/     # Project progress plugin
        ├── .claude-plugin/
        │   └── plugin.json
        └── commands/
```

## Future Plugins

More tools and utilities coming soon! Stay tuned for:
- AI workflow automation
- Code analysis tools
- Documentation generators
- And more...

## Contributing

Contributions are welcome! Feel free to submit issues or pull requests.

## License

MIT License - see LICENSE file for details

## Author

**unclecode**
- GitHub: [@unclecode](https://github.com/unclecode)

## Support

For issues, questions, or feature requests, please open an issue on GitHub.
