# HAProxy collectd Plugin

This [collectd][0] plugin fetches metrics and states from a HAProxy [stats socket][1].

It is a fork of [Fotolia/collectd-mod-haproxy][2]: instead of collecting sessions metrics as counters (`stot` and
`rate`), it collects current sessions (`scur`) as gauges.

## Build Instructions

Note: collectd development headers (e.g. `collectd-dev` package) are required to compile the plugin; slight adjustments
to the `Makefile` might be needed depending on the OS used.

```
make && sudo make install
```

Default plugin installation path is `/usr/lib/collectd`.

## Plugin Configuration

Default configuration settings are:

```
<Plugin "haproxy">
  # Path to HAProxy administration socket
  SocketPath "/var/run/haproxy-stats.sock"

  # Minimum HAProxy instance uptime to reach before collecting metrics
  RestartGap 20

  # Disable bytes metrics collecting
  DisableBytes false

  # Disable denied requests/responses metrics collecting
  DisableDeny false

  # Disable connections/requests/responses metrics collecting
  DisableErrors false

  # Disable current sessions metrics collecting
  DisableSessions false

  # Disable backend server health status metrics collecting
  DisableStatus false

  # Disable HTTP response status codes metrics collecting
  DisableHttpCodes false

  # Trigger collectd notification when a proxy/server is flagged DOWN
  NotifStatusDown false

  # Trigger collectd notification when a proxy/server is flagged back UP
  # (after being flagged DOWN)
  NotifStatusUp false
</Plugin>
```

It is also possible to filter metrics collecting on specific proxies (HAProxy `frontend`) and servers (HAProxy
`backend`):

```
  # Collect metrics for these proxies only
  PxFilter "http_proxy1", "http_proxy2"

  # Collect metrics for these backend servers only
  SvFilter "http_backend1", "http_backend2"

  # Collect frontend-level metrics only (no per-backend metrics)
  SvFilter "FRONTEND"
```

This plugins uses specific collectd [types][3]: either copy the `types.db.haproxy` file and add `TypesDB <path to
types.db.haproxy file>` into your collectd configuration file, or add the types defined in the `types.db.haproxy` file
into your local collectd `types.db` file. Don't forget to propagate these additional types to all your collectd
instances if you do collectd network forwarding.

[0]: https://collectd.org/
[1]: http://cbonte.github.io/haproxy-dconv/configuration-1.5.html#3.1-stats%20socket
[2]: https://github.com/Fotolia/collectd-mod-haproxy
[3]: http://collectd.org/documentation/manpages/types.db.5.shtml
