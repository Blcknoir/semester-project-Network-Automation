# Project Proposal — Network automation using python

Team
- Muhammad Awab — Std ID: F23CSC034
- Abdul Rahman — Std ID: F23CSC028

Project title
- Network automation using python

Course
- Computer Networks — Semester Project

Scope & working criteria
- Use Netmiko (Python scripts) or Ansible (playbooks) to automate:
  - VLAN provisioning across switches
  - ACL management and deployment
  - Interface/IP configuration (assign IP on interfaces / SVI)
- Required features:
  - Secure secrets handling (environment variables, .env, or Ansible Vault)
  - Periodic or on-demand running-config backups with timestamps
  - Dry-run / pre-check mode (Ansible --check / Netmiko dry_run)
  - Post-change verification and parsing (TextFSM or regex)
  - Rollback strategy using backups on validation failure
  - Reproducible run steps in README

Objectives
- Demonstrate device programmability and automation for core network tasks.
- Implement idempotent playbooks (Ansible) or safe scripted flows (Netmiko).
- Provide tests and verification that changes were applied correctly.
- Deliver documentation and a 5–10 minute demo.

Testbed & devices
- Specify the test environment you will use (pick one and document it in README):
  - EVE-NG / GNS3 topology (recommended for safe grading)
  - Physical lab devices (only with instructor approval and snapshots)
  - Container/VM-based routers (FRR, IOSv, vMX, vEOS, etc.)
- Minimum devices for acceptance:
  - 2 switches (for VLAN and ACL tests)
  - 1 router (for interface/IP and routing verification)

Deliverables
- Git repository (tagged release at submission) containing:
  - proposal.md (this file)
  - README.md with reproducible steps
  - inventory/hosts file and sample credentials template (placeholders)
  - scripts/playbooks for backups and change workflows
  - backups/ (sample backup files produced during development)
  - tests/ (simple test harness or pytest scripts)
  - demo video (5–10 min) or slides for live demo
  - final report (PDF) describing architecture, security, test results, and limitations

Timeline summary (6 weeks)
- Week 1: repo, proposal, environment setup, connectivity proof
- Week 2: secrets handling, discovery, backups
- Week 3: core automation (dry-run)
- Week 4: apply change + validation tests
- Week 5: rollback, idempotence, robustness
- Week 6: finalize docs, demo, submit

Acceptance criteria
- Backups saved with timestamps and readable.
- Dry-run shows intended changes without applying.
- Applied change verified by show commands and parsing.
- Rollback restores previous state on simulated failure.
- No plain-text credentials in repo.
- Instructor can reproduce core flows using README.

Risks & mitigations
- Breaking lab devices — use emulators/snapshots and dry-run first.
- Credential leaks — use env vars or Ansible Vault; do a repo scan before submission.
- Time constraints — limit to two device types and a minimal feature set.

Contact / Instructor
- Fill in instructor / TA contact info and preferred demo slot in README.