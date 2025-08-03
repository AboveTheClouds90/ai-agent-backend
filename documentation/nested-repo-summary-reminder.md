# Nested Repo Setup & Submodule Conversion Guide

## What Gemini Did (Current Setup)

Created a **nested independent repository** structure:

```
ai-agent-sandbox/           # Parent repo
├── .git/                   # Parent git history
├── .gitignore              # Contains "ai-agent/"
├── (other sandbox files)
└── ai-agent/               # Nested repo (ignored by parent)
    ├── .git/               # Independent git history
    └── (Spring app files)
```

**Key Points:**
- `ai-agent/` is added to `.gitignore` in the parent repo
- Both repositories are completely independent
- Parent repo ignores the nested repo entirely
- Each repo can be committed/pushed separately

## Current Workflow

```bash
# Work on sandbox
cd ai-agent-sandbox
git add . && git commit -m "Update sandbox"

# Work on backend (separate repo)
cd ai-agent-sandbox/ai-agent  
git add . && git commit -m "Update backend"
```

## How to Convert to Submodules Later

### Prerequisites
- Both repos should be in clean state (no uncommitted changes)
- `ai-agent` must be pushed to a remote repository first

### Conversion Steps

1. **Ensure ai-agent has a remote:**
```bash
cd ai-agent-sandbox/ai-agent
git remote add origin https://github.com/yourusername/ai-agent.git
git push -u origin main
```

2. **Convert in parent repo:**
```bash
cd ai-agent-sandbox

# Remove the nested repo directory
rm -rf ai-agent/

# Remove from .gitignore
# Edit .gitignore and delete the line: ai-agent/

# Add as submodule
git submodule add https://github.com/yourusername/ai-agent.git ai-agent

# Commit the change
git add .gitignore .gitmodules ai-agent
git commit -m "Convert ai-agent to submodule"
```

3. **Team members need to update:**
```bash
git pull
git submodule update --init --recursive
```

## When to Convert to Submodules

**Convert if you need:**
- Version pinning (specific ai-agent commits with specific sandbox commits)
- To share ai-agent across multiple projects
- "Official" git relationship between repos

**Stay with current setup if:**
- Projects are related but independent
- Team wants to avoid submodule complexity
- Simple development workflow is preferred

## Submodule Commands (After Conversion)

```bash
# Clone project with submodules
git clone --recursive https://github.com/user/ai-agent-sandbox.git

# Update submodule to latest
cd ai-agent
git pull origin main
cd ..
git add ai-agent
git commit -m "Update ai-agent submodule"

# Update all submodules
git submodule update --remote
```