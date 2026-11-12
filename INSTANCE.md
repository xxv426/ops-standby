# ops-standby (instance notes)

The ops instance for the standby server: Oracle Cloud VM.Standard.E2.1.Micro
(1/8 OCPU, 1 GB, x86, Ubuntu), Tokyo. Made from
[ops-starter](https://github.com/xxv426/ops-starter); see its README and docs.

It has two jobs, and serves no website:

1. **Watch the main server** with [Gatus](apps/gatus): the sites, and a nightly
   backup heartbeat per app. A server that is down cannot raise its own alert.
2. **Store the main server's backups**: the restic repository
   `/srv/backup/restic`, owned by the `backup-a1` user, which only the main
   server's root key can log in as. It predates ops and is kept as it is; new
   clients get their own user and directory with `ops backup-client add`.

| App | Backups |
|---|---|
| [gatus](apps/gatus) | none: its configuration is in Git, its history is not worth keeping |

## This server

- SSH: `xxvlab-micro` in `~/.ssh/config`. Sync: `scripts/sync.sh xxvlab-micro`
  from Git Bash.
- Private IP 10.0.0.221 (the main server is 10.0.0.50).
- Firewall: only TCP 22 from the internet. TCP 8080 from 10.0.0.50 for
  heartbeats (OCI security list); Gatus binds only the private IP.
- Dashboard: `ssh -L 8080:10.0.0.221:8080 xxvlab-micro`, then http://127.0.0.1:8080.
- `/srv/apps/gatus/.env` holds the SMTP settings and `HEARTBEAT_TOKEN`.
