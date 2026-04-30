# Poolmon

Poolmon is a Dovecot director mailserver pool monitoring script. It is meant to
roughly duplicate health monitoring behavior typically found on dedicated load
balancers (for example LVS or F5 BigIP LTM).

It can be run on more than one director host simultaneously, though differences
in node reachability can cause mailserver vhost count flapping.

For runtime options and protocol checks, run:

```bash
./poolmon --help
```

## Configuration Example

Poolmon reads runtime options from the service environment file as `OPTIONS`.
The systemd unit in this repository uses `/etc/dovecot/poolmon.conf`.

Example `/etc/dovecot/poolmon.conf`:

```bash
OPTIONS="--socket /var/run/dovecot/director-admin \
  --interval 30 \
  --timeout 5 \
  --port IMAP:143 \
  --ssl IMAP:993 \
  --port POP3:110 \
  --ssl POP3:995 \
  --logfile syslog \
  --credfile /etc/dovecot/poolmon.creds"
```

Example credentials file `/etc/dovecot/poolmon.creds` (mode `0600`):

```text
healthcheck-user
super-secret-password
```

To run manually in foreground while testing:

```bash
./poolmon -f --socket /var/run/dovecot/director-admin --port IMAP:143 --ssl IMAP:993
```

## Contributors

The following contributors were collected from:

```bash
git shortlog -sne --all
```

- Brandon Davidson <brandond@uoregon.edu>
- Carsten Rosenberg <c.rosenberg@heinlein-support.de>
- Brandon Davidson <brad@oatmail.org>
- David Warden <dfwarden@gmail.com>
- c-rosenberg <33958273+c-rosenberg@users.noreply.github.com>
- georgeto <georgeto@mailbox.org>
- Brad Davidson <brad@oatmail.org>
- Peter Wienemann <peter.wienemann@uni-bonn.de>
- Manu Zurmühl <m.zurmuehl@heinlein-support.de>
- Mikal Sande <mikal.sande@altibox.no>
- Timo Sirainen <timo.sirainen@dovecot.fi>

## Original Project and Fork Lineage

Based on local git history and configured remotes:

- Earliest commit in this repository is by Brandon Davidson (`first commit`,
  2010-08-19), indicating the original project starts there.
- Current fork remote:
  - https://github.com/HeinleinSupport/poolmon.git
- Configured upstream remote:
  - https://github.com/agdsn/poolmon.git

Notes:

- Historic packaging references in the RPM spec also point to the original
  Brandon Davidson GitHub namespace, which matches the earliest commit history.
- If you want this section to track changes automatically, regenerate it with:

```bash
git shortlog -sne --all
git remote -v
git log --reverse --format='%h %ad %an <%ae> %s' --date=short | head
```
