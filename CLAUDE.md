# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a test repository for Claude Code on the web, running in a secure sandboxed environment.

## Current State

The repository is currently empty except for configuration files. It serves as a workspace for:
- Testing Claude Code on the web features
- Cloning and exploring external repositories (which are gitignored)
- Experimenting with sandboxed development environments

## Working with External Repositories

External repositories cloned into this workspace are excluded from version control via `.gitignore`. This allows exploration of external codebases without committing them to this repository.

## Git Branch Convention

Development branches follow the pattern: `claude/<session-id>`
- All development work should occur on session-specific branches
- Branches must start with `claude/` prefix
- Push operations require the correct session ID suffix
