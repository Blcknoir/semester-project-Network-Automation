
# Network automation using python

Team
- Muhammad Awab — Std ID: F23CSC034
- Abdul Rahman — Std ID: F23CSC028

Project summary
- Automate VLAN provisioning, ACL management, and interface/IP setup using Netmiko (Python) or Ansible.
- Provide secure secrets handling, running-config backups, dry-run/pre-checks, post-change verification and rollback.

Quick structure
- proposal.md
- 6-week-roadmap.md
- README.md (this file)
- inventory/                # Ansible inventory or device list
- scripts/                  # Netmiko scripts and helpers
- playbooks/                # Ansible playbooks
- backups/                  # Generated running-config backups
- tests/                    # Simple verification tests (pytest or scripts)
- docs/                     # report, slides, demo video

Requirements
- Python 3.8+
- virtualenv (recommended)
- For Netmiko: pip install netmiko
- For Ansible: pip install ansible ansible-core && ansible-galaxy collection install cisco.ios (or other vendor collections)

Quick setup (example)
1. Create and activate venv:
   python -m venv .venv
   source .venv/bin/activate

2. Install dependencies:
   # Netmiko-only
   pip install netmiko
   # or Ansible-only
   pip install ansible ansible-core
   ansible-galaxy collection install cisco.ios

Secrets (do NOT commit real credentials)
- Recommended: use environment variables or Ansible Vault.
  - Environment vars example:
    export NET_USER=student
    export NET_PASS=changeme
- Or create a .env.template with placeholders and fill a private .env (excluded by .gitignore).
- For Ansible: use `ansible-vault create vault.yml` or pass --extra-vars at runtime.

Connectivity proof (instructor should be able to run this)
- Netmiko:
  - Ensure NET_USER and NET_PASS are set.
  - Run the sample connectivity/dry-run script:
    python scripts/sample_netmiko.py
    (script prints connections and runs in dry-run by default — see script header)

- Ansible (example ad-hoc show command — adjust modules for vendor):
  - Confirm inventory at inventory/hosts or inventory.ini
  - Run a simple read-only command via a playbook or ad-hoc:
    ansible-playbook -i inventory.ini playbooks/sample_ansible_playbook.yml --tags backup --extra-vars "ansible_user=USER ansible_password=PASS"
  - Dry-run:
    ansible-playbook -i inventory.ini playbooks/sample_ansible_playbook.yml --check --diff --extra-vars "ansible_user=USER ansible_password=PASS"

Backups
- Backups are stored in the backups/ directory with timestamps:
  backups/network-automation-using-python-<host>-YYYYMMDDTHHMMSS.cfg
- To create backups:
  - Netmiko: python scripts/sample_netmiko.py (backup section runs automatically)
  - Ansible: run playbook with backup task or use ios_command tasks and local copy to backups/

Core workflows (dry-run then apply)
- Dry-run first:
  - Ansible: --check --diff
  - Netmiko: run script default dry_run=True (prints commands)
- Apply (only after instructor approval for lab devices):
  - Ansible: ansible-playbook -i inventory.ini playbooks/your_workflow.yml --extra-vars "@secrets.yml"
  - Netmiko: python scripts/your_script.py --apply

Validation & tests
- Each workflow includes post-change verification (show commands parsed with TextFSM/regex).
- Run tests located in tests/:
  - pytest tests/    (if pytest-based)
  - or python tests/validate_vlan.py

Rollback
- Rollback uses the latest backup file for the host.
- To trigger rollback (demo):
  - Netmiko: python scripts/rollback.py --host <host>
  - Ansible: run playbook playbooks/rollback.yml --extra-vars "ansible_user=USER ansible_password=PASS"

Acceptance checklist (for instructor)
- Connectivity proof runs and logs results.
- Backups exist for ≥2 devices with timestamps.
- Dry-run shows intended CLI without changing devices.
- Apply + Validate: show commands confirm VLAN/ACL/IP state.
- Rollback restores pre-change configuration on simulated failure.
- No plaintext credentials in the repo.

Developer / student workflow notes
- Work in feature branches: feature/<task-name> and open PRs for reviews.
- Keep commits small and meaningful.
- Use .gitignore to exclude .env and real backup artifacts if large.
- Tag final submission: v1.0-submission

Contacts & demo
- Prepare a 5–10 minute demo showing:
  1. Backup generation
  2. Dry-run of intended change
  3. Apply change and show validation output
  4. Simulate failure and perform rollback
  5. Run idempotence check (run playbook/script twice)

If you want, I can also add:
- a ready-to-use secrets-template (.env.template),
- a starter scripts/sample_netmiko.py and playbooks/sample_ansible_playbook.yml already adjusted for this repo,
- or create GitHub issues for the 6-week tasks so you can track progress.
```
