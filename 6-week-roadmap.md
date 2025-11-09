# 6‑Week Roadmap — Network automation using python
Team: Muhammad Awab (F23CSC034) and Abdul Rahman (F23CSC028)

Overview
- Goal: Deliver a reproducible automation workflow (Netmiko or Ansible) covering VLAN provisioning, ACL management, and interface/IP configuration with backups, validation, and rollback.

Week 1 — Kickoff: proposal, repo & environment
- Goals:
  - Finalize scope, tool choice (Netmiko or Ansible), and topology.
  - Create Git repo skeleton and connectivity proof.
- Tasks (both):
  - Complete and commit `proposal.md` (this file) to repo.
  - Create README.md skeleton, .gitignore, backups/ directory.
  - Set up Python virtualenv and install required packages (ansible or netmiko).
  - Build or obtain lab topology (EVE-NG/GNS3 or device access).
  - Produce a connectivity proof (SSH ping or Ansible ad-hoc) and save logs.
- Deliverable: proposal.md, README stub, connectivity log (connectivity-proof.txt).
- Ownership:
  - Muhammad: repo init, README/branch structure.
  - Abdul: setup testbed/emulator and produce connectivity proof.
- Acceptance:
  - Instructor can follow README to run connectivity test.

Week 2 — Secrets handling, discovery & backups
- Goals:
  - Implement secure credential method and automated backups.
- Tasks:
  - Decide secrets approach: environment variables, .env + python-decouple, or Ansible Vault.
  - Implement device discovery/gather-facts (hostname, model, interfaces).
  - Implement running-config backup script/playbook that writes to backups/<project>-<host>-TIMESTAMP.cfg.
  - Document how to provide credentials (secrets-template.md).
- Deliverable: backup script/playbook, sample backup files, secrets-template.md.
- Ownership:
  - Muhammad: implement backup script/playbook (Netmiko/Ansible).
  - Abdul: implement discovery/gather-facts and document secrets usage.
- Acceptance:
  - Backups generated for at least 2 devices and stored with timestamp filenames.
  - No real credentials committed.

Week 3 — Core automation (dry-run & pre-checks)
- Goals:
  - Implement core workflows with dry-run/pre-check capability.
- Tasks:
  - Implement at least one workflow for each feature area (VLANs, ACLs, IP setup) in a minimal form:
    - VLAN provisioning from CSV or YAML input.
    - ACL push from templated file.
    - Interface IP assignment from variables.
  - Add pre-checks: verify device state before changes.
  - Implement dry-run: Ansible `--check/--diff` or Netmiko `dry_run=True` to print intended CLI.
  - Create sample input files (csv/, vars/).
- Deliverable: scripts/playbooks + sample inputs + dry-run logs.
- Ownership:
  - Muhammad: VLAN workflow + dry-run for VLANs.
  - Abdul: ACL and interface/IP workflow + pre-checks.
- Acceptance:
  - Dry-run produces intended CLI commands and does not change devices.

Week 4 — Apply change & validate
- Goals:
  - With approval, apply changes and implement automated verification.
- Tasks:
  - Apply changes to the lab (get instructor approval).
  - Implement post-change verification tasks: run show commands, parse outputs (TextFSM or regex), and assert expected state.
  - Add at least one automated test (pytest wrapper or simple assert script).
- Deliverable: apply-run logs, verification outputs, tests in tests/.
- Ownership:
  - Muhammad: run apply for VLANs, capture logs, write VLAN verification test.
  - Abdul: run apply for ACL/IP tasks, capture logs, write ACL/IP verification test.
- Acceptance:
  - Verification scripts confirm the expected config lines exist and tests pass.

Week 5 — Rollback, idempotence & robustness
- Goals:
  - Implement rollback and ensure repeatable, robust runs.
- Tasks:
  - Implement rollback using backups or explicit revert steps and document how to invoke it.
  - Demonstrate idempotence:
    - Ansible: run playbook twice → second run reports zero changes for config tasks.
    - Netmiko: script must detect existing config and avoid duplicates.
  - Add error handling, retries, and structured logging.
  - Simulate failure to test rollback path and save logs.
- Deliverable: rollback script, idempotence logs (two successive run outputs), enhanced logs with timestamps.
- Ownership:
  - Muhammad: implement rollback for VLAN changes and idempotence validation.
  - Abdul: implement rollback for ACL/IP and improve logging/error handling.
- Acceptance:
  - Simulated failure triggers rollback and device returns to backup state.
  - Two consecutive runs are safe (no duplicate config, or Ansible changed=0).

Week 6 — Finalize: docs, demo & submission
- Goals:
  - Final documentation, demo video, and submission bundle.
- Tasks:
  - Write final report (PDF) describing design decisions, security, testing, and limitations.
  - Finalize README with reproducible steps, sample secrets usage, and how to run tests.
  - Record a 5–10 minute demo video demonstrating: backup, dry-run, apply+validate, rollback, idempotence.
  - Tag or zip repo for submission (e.g., v1.0-submission).
- Deliverable: final report, README, demo video, tests passing, tagged release or zip file.
- Ownership:
  - Muhammad: prepare demo video and final report section on VLAN workflow and results.
  - Abdul: finalize README and report sections on ACL/IP and testing; prepare submission zip/tag.
- Acceptance:
  - Instructor can reproduce the core flows following README and acceptance checklist.

Milestones & grading weights
- End W1: Repo + proposal + connectivity — 10%
- End W2: Backups + secrets handling — 20%
- End W4: Core automation + validation — 40%
- End W6: Rollback + idempotence + report + demo — 30%

Instructor test checklist (must pass)
- Connectivity failure: script/playbook logs error and continues to other devices.
- Dry-run: dry-run shows intended changes without applying them.
- Apply + Validate: show commands demonstrate VLANs/ACLs/IP set as expected.
- Rollback: failure during validation restores device config to pre-change backup.
- Idempotence: second run produces zero changes or safe no-op behavior.
- Repo contains no real credentials.

Practical commands & examples
- Ansible:
  - Install: python -m pip install ansible
  - Dry-run: ansible-playbook -i inventory.ini playbook.yml --check --diff
  - Idempotence check: ansible-playbook -i inventory.ini playbook.yml && ansible-playbook -i inventory.ini playbook.yml
  - Vault: ansible-vault create secrets.yml
- Netmiko pattern (python):
  - virtualenv: python -m venv .venv && source .venv/bin/activate
  - install: pip install netmiko
  - implement dry_run flag; print commands in dry-run and only send when dry_run=False
- Backup filename convention:
  - backups/network-automation-using-python-<host>-YYYYMMDDTHHMMSS.cfg

Estimated hours (per student per week)
- Week 1: 6–8 h each
- Week 2: 8–10 h each
- Week 3: 10–12 h each
- Week 4: 8–12 h each
- Week 5: 8–12 h each
- Week 6: 6–8 h each

Notes & tips
- Keep commits small and frequent; use branches feature/<task> and open a PR for instructor review.
- Do not commit credentials — use placeholders and a secrets-template.md (example: .env.template).
- Keep logs and backups in the repository only as sample artifacts; real backup artifacts can be large — optionally store as release artifacts or a shared drive.

Quick next steps (do now)
- Commit `proposal.md` and `6-week-roadmap.md` to project root.
- Create README with "How to run connectivity proof" section and add connectivity log.
- If you want, I can generate:
  - GitHub issue list (6 weekly issues with task checklists) ready to paste into Issues,
  - A starter sample_netmiko.py or sample_ansible_playbook.yml renamed for this project,
  - secrets-template.md and README content.
```