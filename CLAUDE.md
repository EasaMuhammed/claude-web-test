# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a test repository for Claude Code on the web, running in a secure sandboxed environment. It serves as a workspace for:
- Testing Claude Code web features and capabilities
- Cloning and exploring external repositories
- Experimenting with sandboxed development workflows

## Current Contents

**nanochat/** - Cloned repository of Andrej Karpathy's nanochat project (a minimal ChatGPT implementation). See `nanochat/CLAUDE.md` for detailed documentation on working with nanochat.

## Git Branch Convention

Development branches follow the pattern: `claude/<session-id>`
- All development work should occur on session-specific branches
- Branches must start with `claude/` prefix
- Push operations require the correct session ID suffix

## Working with External Repositories

External repositories cloned into this workspace are excluded from version control via `.gitignore`. This allows exploration without committing external codebases to this repository.
