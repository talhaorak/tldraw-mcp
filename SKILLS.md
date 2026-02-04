#!/usr/bin/env bash
set -euo pipefail

# OpenClaw Skill Installer for @talhaorak/tldraw-mcp
# Usage:
#   curl -fsS https://raw.githubusercontent.com/talhaorak/tldraw-mcp/main/SKILLS.md | bash

REPO_RAW_BASE="https://raw.githubusercontent.com/talhaorak/tldraw-mcp/main"
TARGET_DIR="${HOME}/clawd/skills/tldraw-mcp"

mkdir -p "$TARGET_DIR"

curl -fsS "$REPO_RAW_BASE/openclaw-skill/SKILL.md" -o "$TARGET_DIR/SKILL.md"

echo "Installed OpenClaw skill to: $TARGET_DIR"
echo "You may need to restart OpenClaw to pick up new skills."