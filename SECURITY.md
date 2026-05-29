# Security Policy

## Reporting a Vulnerability

If you find a Skill that contains malicious code, or discover a security vulnerability in this repository:

1. **Do NOT open a public Issue** for security vulnerabilities.
2. Use the [flag issue template](https://github.com/hikari-2424/awesome-reasonix/issues/new?template=skill-flag.yml) to report harmful Skills.
3. For sensitive vulnerabilities in the repository infrastructure itself, contact the maintainers directly.

## Safety Guidelines for Users

Skills are markdown files containing instructions for an AI agent that has access to your terminal, filesystem, and network. **Always review a Skill before using it.**

### What to check before installing a Skill:

- **Read the full Skill file.** It's markdown — you can read it on GitHub before downloading.
- **Check what commands it runs.** Look for `run_command`, `bash`, `powershell`, or shell code blocks.
- **Verify the author.** Check their GitHub profile and history.
- **Check reviews and flags.** Community ratings and reports are visible in `reviews/` and `flags/`.
- **Test in a safe environment first.** If unsure, run the Skill on a test project or in a container.

### Skills CAN do:

- Read and edit files in your project
- Execute shell commands
- Make network requests
- Install packages
- Access any tool available to Reasonix Code

### Skills CANNOT do (unless you explicitly allow):

- Access files outside your project directory
- Run without your confirmation (if you have permission prompts enabled)
- Override Reasonix Code's built-in safety guards

## For Skill Authors

- Disclose what your Skill does upfront. Be specific about commands it runs.
- Use `subagent` mode (`runAs: subagent`) for Skills that need web access or intensive processing.
- Never include hardcoded credentials, tokens, or API keys in Skill files.
- Test your Skill before submitting. Broken Skills waste everyone's time.
- If your Skill modifies files, make it preview changes before applying.

## Automated Security Checks

The CI workflow (`.github/workflows/validate.yml`) automatically validates:

- All linked files exist
- All Skill files have required frontmatter (`name`, `description`)
- `registry.json` is valid JSON

Community members can flag problematic Skills using the flag system. Moderators review flags and may remove or annotate flagged entries.

---

**This repository is community-maintained. Skills are contributed by third parties and are not endorsed by any organization. Use at your own discretion.**
