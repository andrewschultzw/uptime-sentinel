# uptime-sentinel

External vantage point for the homelab's public endpoints, running on GitHub
Actions every ~5 minutes (GitHub may stretch the cron under load). Exists
because every in-lab monitor (Prometheus, Uptime Kuma) resolves public
hostnames to LAN addresses via the internal DNS rewrite — none of them can see
whether the Cloudflare tunnel path actually works from the internet.

- Probes `schultzsolutions.tech` and the public ntfy health endpoint, 3 tries each.
- On failure, alerts via **ntfy.sh** (the public SaaS instance — deliberately NOT
  the self-hosted ntfy, which would be down in exactly the scenario this detects).
  Topic name lives in the `NTFY_EXTERNAL_TOPIC` repo secret.
- `workflow_dispatch` with `simulate_failure: true` tests the alert path end to end.

This repo is public so the scheduled workflow costs no Actions minutes. It
contains only already-public hostnames.
