# Codex Hyper-V Lab

![Codex Hyper-V Lab](images/00_project_hero/codex_hyperv_lab_hero.png)

This repository documents a controlled Codex CLI test in an isolated Hyper-V virtual machine.

The purpose of the project is not to build a production development environment. It is a learning and documentation lab that explores how a local coding agent can be tested safely with manual approvals, synthetic data, isolated workspaces, and reversible VM checkpoints.

## Key principles

- dedicated Windows 11 virtual machine;
- internet access through an OPNsense EDGE NAT layer;
- no direct host-to-guest file sharing;
- no personal or workplace data;
- separate standard local Windows user;
- manual Hyper-V checkpoints;
- synthetic test files only;
- manual approval before Codex changes;
- independent verification outside Codex.

## Current status

The first controlled test has been completed. Codex created a single `hello.txt` file in an isolated workspace, requested approval before making changes, and the result was independently verified in PowerShell.

The documentation is primarily written in Hungarian. File and folder names are kept in English for repository consistency.

## Documentation

- `docs/01_environment_setup.md`
- `docs/02_command_log.md`
- `docs/03_first_controlled_test.md`
- `docs/04_findings_and_next_steps.md`

## Note

This repository does not contain passwords, API keys, GitHub tokens, session identifiers, workplace files, or real data.

## Related project

This lab is one preparatory part of a larger portfolio workflow.

The `codex-hyperv-lab` repository documents how I set up an isolated and controlled Codex CLI test environment inside a Hyper-V virtual machine. The multi-database foundation work is documented in a separate repository:

- [multi-rdbms-comparison-lab](https://github.com/egyprogramozo/multi-rdbms-comparison-lab)

The next practical project builds on both of these foundations:

- [multi-rdbms-data-extraction-lab](https://github.com/egyprogramozo/multi-rdbms-data-extraction-lab)

The goal of `multi-rdbms-data-extraction-lab` is to document controlled data extraction from multiple RDBMS sources and one manual CSV source. In that workflow, Codex is used as a controlled and isolated development assistant, not as an autonomous decision-making system.
