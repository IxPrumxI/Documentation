import ConfigLink from '@site/src/components/ConfigLink';

# Member caching

DiscordSRV keeps Discord member data in memory so features such as role synchronization, nickname synchronization, and account linking can use it. Member caching is configured in the <ConfigLink file="config" path="member-caching">`member-caching`</ConfigLink> section of `config.yml`.

:::warning
Member caching requires Discord's **Server Members Intent**. Enable it for the bot in the [Discord Developer Portal](https://discord.com/developers/applications), and make sure DiscordSRV is allowed to use it.
:::

## Options

| Option | Default | Description |
|---|---:|---|
| <ConfigLink file="config" path="member-caching.enabled">`enabled`</ConfigLink> | `true` | Whether DiscordSRV caches members. When `false`, no members are cached and the other member-cache filters have no effect. |
| <ConfigLink file="config" path="member-caching.chunk">`chunk`</ConfigLink> | `true` | Whether DiscordSRV requests the members of selected servers when it starts. This requires the Server Members Intent. |
| <ConfigLink file="config" path="member-caching.lru">`lru`</ConfigLink> | `5000` | Maximum number of members kept per server using a least-recently-used policy. Set to `0` to keep all members instead of applying an LRU limit. |
| <ConfigLink file="config" path="member-caching.role-ids">`role-ids`</ConfigLink> | `[]` | Discord role IDs to use as a cache filter. If any IDs are configured, only members with at least one of those roles are kept. An empty list keeps all members, subject to the other options. |
| <ConfigLink file="config" path="member-caching.chunking-server-filter">`chunking-server-filter`</ConfigLink> | | Controls which servers are loaded at startup when `chunk` is enabled. |

### `chunking-server-filter`

| Option | Default | Description |
|---|---:|---|
| <ConfigLink file="config" path="member-caching.chunking-server-filter.blacklist">`blacklist`</ConfigLink> | `true` | If `true`, the listed server IDs are excluded from startup loading. If `false`, only the listed server IDs are loaded at startup. |
| <ConfigLink file="config" path="member-caching.chunking-server-filter.ids">`ids`</ConfigLink> | `[]` | Server IDs used by the filter. With the default blacklist mode, an empty list includes every server. In whitelist mode, an empty list includes no servers at startup. |

The startup filter only affects the initial member load. Servers excluded from startup loading can still receive member updates after the bot connects.

## Examples

Cache every member, without an LRU limit:

```yaml
member-caching:
    enabled: true
    lru: 0
```

Cache any number of members with a linked role. Replace the example ID with the role ID from your Discord server:

```yaml
member-caching:
    enabled: true
    lru: 0
    role-ids:
    - 123456789012345678
```

This is useful with [Linked Role](/ascension/synchronization/linked-role), where the configured role is assigned to linked users.

Load members at startup only for selected servers:

```yaml
member-caching:
    enabled: true
    chunking-server-filter:
        blacklist: false
        ids:
        - 123456789012345678
```
