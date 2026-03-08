---
name: update-readme
description: Generate or update project README file. Use when the user asks to "update README", "generate README", "refresh README", or wants to create project documentation that reflects the current project structure. This skill scans the project directory and creates well-structured README content.
---

# Update README Skill

This skill automatically generates or updates the project's README.md file based on the current directory structure.

## When to Use

Use this skill when the user wants to:
- Create a new README.md
- Update an existing README.md
- Refresh project documentation to reflect current structure
- Generate project overview documentation

## Core Functionality

### 1. Scan Project Structure

First, read the current directory structure to understand what folders and files exist:
- Use the Glob tool to find all directories
- Get an overview of key files and folders

### 2. Exclude Specific Folders and Files

**Always exclude these from the README**:

**Standard exclusions:**
- `node_modules/` - package dependencies
- `.git/` - version control data
- Any build directories (e.g., `dist/`, `build/`)

**Read .gitignore and exclude its contents:**
- Parse the `.gitignore` file if it exists
- Exclude all files and folders listed in `.gitignore`
- Example: if `.gitignore` contains `temp/*`, exclude the `temp/` folder

**Specific files to exclude:**
- `CLAUDE.md` - project instructions (not part of user-facing documentation)

### 3. Special Handling for `.claude/` Folder

The `.claude/` folder contains agents and skills that need special documentation:

**For Agents** (files in `.claude/agents/`):
- Read the first 5-10 lines of each agent's `.md` file
- Extract the purpose from the frontmatter or description
- Write a brief description (20 characters or less)
- Format: `- **agent-name**: Brief description of purpose`

**For Skills** (folders in `.claude/` with `SKILL.md`):
- Read the `name` and `description` from the SKILL.md frontmatter
- Write a brief description (20 characters or less)
- Format: `- **skill-name**: Brief description of purpose`

**Do NOT document**:
- Sub-agents (they are internal implementation details)
- Scripts, assets, or other supporting files

### 4. Document Other Folders

For all other folders in the project:
- Provide a brief overview of the folder's purpose
- Mention the type of content (e.g., "documentation files", "configuration files")
- **Do NOT list individual files** - keep it high-level
- Format: `- **folder-name/**: Brief description of contents`

## Output Format

Structure the README.md as follows:

```markdown
# [Project Name]

[Brief project description if available from existing README or infer from structure]

## Project Structure

### .claude/

AI configuration and tools:

**Agents:**
- **agent-name**: Description (20 chars max)

**Skills:**
- **skill-name**: Description (20 chars max)

### [Other Folders]

- **folder-name/**: Description of contents

**Note**: Excludes folders/files from `.gitignore` and `CLAUDE.md`

## [Other Existing Sections]

[Preserve any other sections from existing README that don't conflict]
```

## Implementation Steps

1. **Check for existing README**
   - Use Read tool to read `readme.md` or `README.md` if it exists
   - Preserve project name, description, and non-structure sections

2. **Read exclusion rules**
   - Read `.gitignore` file if it exists
   - Parse all patterns (folders and files to exclude)
   - Add to exclusion list: `node_modules/`, `.git/`, `CLAUDE.md`, and all `.gitignore` entries

3. **Scan directory structure**
   - Use Glob to find all top-level folders
   - Filter out excluded folders based on combined exclusion list

4. **Process .claude/ folder**
   - List all agents in `.claude/agents/`
   - List all skills (folders with SKILL.md)
   - Read frontmatter to get descriptions
   - Summarize each in 20 characters or less

5. **Document other folders**
   - For each remaining folder, provide brief description
   - Keep descriptions concise and informative

6. **Generate final README**
   - Combine all sections
   - Use Edit tool to update existing README or Write tool to create new one
   - Ensure proper markdown formatting

## Example Output

```markdown
# DHC AI Guidebook

Claude Code usage documentation and best practices.

## Project Structure

### .claude/

AI configuration and tools:

**Agents:**
- **doc-category-checker**: 验证文档分类规范
- **doc-cross-duplicate-checker**: 检查重复内容

**Skills:**
- **skill-creator**: 创建和优化技能

### claude-code-usage-guidebook/

Documentation files covering Claude Code usage from beginner to advanced topics.
```

**Note**: Folders like `temp/` (from .gitignore) and files like `CLAUDE.md` are excluded from documentation.

## Important Notes

- Always use relative paths when referencing files
- Maintain markdown formatting consistency
- Keep descriptions brief and informative
- If unable to infer purpose, use generic but accurate descriptions
- Preserve user's custom sections when updating existing README
- **Always read and respect `.gitignore`**: Parse it and exclude all listed files/folders
- **Never document `CLAUDE.md`**: This is internal project configuration, not user-facing content
