# DNS Resolution Workaround for Claude Code Web

## Problem

Claude Code Web VMs have broken DNS resolution (see [GitHub Issue #14538](https://github.com/anthropics/claude-code/issues/14538)). The environment provides an HTTP proxy for network access, but Java/Scala build tools fail because they attempt DNS resolution before connecting to the proxy.

## Root Cause

1. `/etc/resolv.conf` is empty (no nameservers configured)
2. Java's `InetAddress` and Coursier try DNS resolution before proxy connection
3. curl works because it connects to proxy first without DNS: `http_proxy=http://...@21.0.0.135:15004`

## Solution

Add Maven Central and other required hosts to `/etc/hosts`:

```bash
# Add to /etc/hosts
echo "151.101.112.209 repo1.maven.org" >> /etc/hosts
echo "185.199.108.153 github.com raw.githubusercontent.com" >> /etc/hosts
```

## Verification

```bash
# Test DNS resolution
java -cp . TestDNS

# Expected output:
# repo1.maven.org -> 151.101.112.209
# github.com -> 185.199.108.153
```

## Build Tool Status

### Mill
- ✅ Partially working with /etc/hosts entries
- ⚠️ Very slow downloads through proxy
- Downloads artifacts but may timeout on first run

### Bleep
- ❌ Not yet tested with workaround
- Should work with same /etc/hosts entries

## Additional Hosts Needed

You may need to add more entries for:
- `oss.sonatype.org` - Sonatype snapshots/releases
- `jcenter.bintray.com` - JCenter repository
- `plugins.gradle.org` - Gradle plugins
- CDN endpoints for Fastly/Cloudflare

## Proper Fix

This issue requires a fix from Claude Code Web infrastructure to either:
1. Configure DNS properly in the VM
2. Configure Java/Coursier to use the proxy without DNS
3. Provide a transparent DNS proxy

## References

- [GitHub Issue #14538](https://github.com/anthropics/claude-code/issues/14538)
- [Coursier Proxy Docs](https://get-coursier.io/docs/other-proxy)
- [Java DNS Resolution Issues](https://github.com/netty/netty/issues/6844)
