# github-skills

This repo contains 2 skills for use with Claude Skills, gh-cli and github-actions-writer.
They aim to simplify interacting with the Github API via the gh CLI, for use in Claude Code.
The gh CLI handles all authentication, so you don't have to worry about me leaking your keys,
this was entirely made by Claude so use at your own risk.

Please please PLEASE be mindful of the risks involved in pulling arbitrary code from Github,
the most obvious risk is prompt injection but arbitrary code execution isn't great either.
Try to avoid those

## gh-cli

This skill contains 3 scripts made to automate repeatitive github workflows i found myself doing,
even with Claude doing it all it was still wasteful. The SKILL.md is limited, deferring to the references.
review the SKILL.md and references for more info

- gh_code_search.py - searches for code on Github, such as searching your codebase for leaked credentials, or searching for examples
- gh_failed_run.py - extracts the error message from your most recent Github Actions run
- gh_pages_deploy.py - handles the tedium of deploy to Github Pages

## github-actions-writer

This one is as much for me as Claude. Claude already knows how to write Github Actions (he wrote the skill!),
but it has some templates and best practices.


## Dependencies

- Python 3.9+
- gh CLI

## Setup

- Install the gh CLI, in your terminal run 'gh auth login' and follow the instructions.
- Download the skill you want, then put it in ./claude/skills/, and restart Claude Code.

## Usage

- Run 'gh auth login' and follow the instructions.
- Telling Claude to explicitly use the skill tends to work best.
- Use the official 'Skills Creator' skill and make your own if you want,
nothing special about mine