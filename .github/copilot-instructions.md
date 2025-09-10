# ElevenLabs Agents CLI Demo

ElevenLabs Agents CLI Demo is a conversational AI agent management project using the ElevenLabs Agents platform. It manages voice and text-based AI agents through configuration files and deploys them using the ConvAI CLI.

Always reference these instructions first and fallback to search or bash commands only when you encounter unexpected information that does not match the info here.

## Working Effectively

### Bootstrap and Setup
- Install ConvAI CLI globally: `npm install -g @elevenlabs/convai-cli` -- takes 40 seconds. NEVER CANCEL. Set timeout to 5+ minutes.
- Check CLI version: `convai --version`
- Verify installation: `convai --help`

### Authentication
- Check login status: `convai whoami`
- Login with API key: `convai login` (interactive prompt for ElevenLabs API key)
- Alternatively, set environment variable: `export ELEVENLABS_API_KEY=your_api_key_here`
- Logout: `convai logout`

### Core Agent Management
- List configured agents: `convai list-agents` -- takes <1 second
- Check agent status: `convai status --env prod` -- takes <1 second
- Preview changes: `convai sync --env prod --dry-run` -- takes <1 second. SAFE to run without API key.
- Deploy agents: `convai sync --env prod` -- REQUIRES API key. Takes <5 seconds with valid auth.
- Deploy all environments: `convai sync` -- REQUIRES API key. Takes <10 seconds with valid auth.

### Project Management
- Initialize new project: `convai init [path]` -- takes <1 second
- Add new agent: `convai add agent "Agent Name" --template default`
- Available templates: `convai templates list`
- Show template config: `convai templates show <template>`

### Development Workflow
- Watch for config changes: `convai watch --env prod --interval 5`
- Generate widget code: `convai widget "Agent Name"`
- Validate configs: All JSON files are automatically validated. Use `convai sync --dry-run` to check for issues.

## Validation

### Manual Testing Scenarios
Always manually validate agent functionality after making changes:
1. Verify agent configuration loads correctly: `convai status --env prod`
2. Test configuration syntax: `convai sync --env prod --dry-run`
3. Check widget generation: `convai widget "Store Assistant"`
4. Validate JSON files: `python3 -m json.tool agent_configs/store_assistant.json`

### CI/CD Pipeline
The repository uses GitHub Actions (.github/workflows/main.yml) that automatically:
1. Installs ConvAI CLI
2. Validates configurations with dry-run
3. Deploys to production environment
4. Verifies deployment status

Always ensure your changes pass dry-run validation before committing: `convai sync --env prod --dry-run`

## Common Tasks

### Repository Structure
```
ls -la /
.env.example          # Environment variables template
.github/              # CI/CD workflows
README.md            # Basic project documentation
agent_configs/       # Agent configuration files
  store_assistant.json # Store Assistant agent config
agents.json          # Agent registry
convai.lock          # Deployment lock file (tracks deployed agents)
tools.json           # Tools configuration (currently empty)
```

### Key Configuration Files

#### agents.json
```json
{
    "agents": [
        {
            "name": "Store Assistant",
            "config": "agent_configs/store_assistant.json"
        }
    ]
}
```

#### tools.json
```json
{
    "tools": []
}
```

#### convai.lock
Tracks deployed agent IDs and configuration hashes. DO NOT edit manually.

### Troubleshooting Common Issues

#### Authentication Issues
- Error: "No API key found" → Run `convai login` or set `ELEVENLABS_API_KEY` environment variable
- Error: "Not logged in" → Check `convai whoami` and login if needed

#### Configuration Issues
- Error: "Invalid JSON" → Validate JSON syntax with `python3 -m json.tool <file>`
- Error: "agents.json not found" → Run `convai init` to initialize project
- Error: "Config file not found" → Check file paths in agents.json

#### Deployment Issues
- Agent shows "No changes" → Configuration matches deployed version
- Sync fails → Check API key and network connectivity
- Status shows "Not synced" → Run `convai sync --env prod` to deploy

### Working with Agent Configurations
- The Store Assistant agent is configured for customer service scenarios
- It includes voice settings, conversation parameters, and webhook integrations
- Agent supports multiple languages (English, Spanish, Portuguese)
- Webhook URL: https://v0-ecommerce-business-page.vercel.app/api/orders/{order_id}

### Best Practices
- Always run `convai sync --dry-run` before actual deployment
- Use version control for all configuration changes
- Test configuration changes in development environment first
- Keep sensitive data in environment variables, not configuration files
- Validate JSON syntax before committing changes

### Environment Notes
- Primary environment: `prod`
- ConvAI CLI version: 0.2.2
- Node.js required for CLI installation
- No additional build steps or compilation required
- All operations are file-based configuration management

### Command Timing Expectations
- CLI installation: 40 seconds (NEVER CANCEL - use 5+ minute timeout)
- All local operations (list, status, dry-run): <1 second
- Deployment operations: <10 seconds with valid authentication
- Project initialization: <1 second

### Manual Validation Requirements
After making any configuration changes, ALWAYS:
1. Run `convai sync --env prod --dry-run` to validate syntax
2. Check `convai status --env prod` for deployment status
3. Generate widget code with `convai widget "Store Assistant"` to verify agent accessibility
4. Validate all JSON files with appropriate tools