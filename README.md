# NextUp Walk-in MCP server

Live walk-in waits for AI assistants. This is a hosted, public, read-only
[Model Context Protocol](https://modelcontextprotocol.io) server from
[NextUp Walk-in](https://nextupwalkin.com). It finds barbers, salons, groomers and other
walk-in businesses in the US, reads each one's live wait, and gives the person a link to join
the line or book a time.

This repository holds the server's public listing and documentation. The server runs on
NextUp Walk-in's infrastructure, so there is nothing to install or run yourself.

| | |
| --- | --- |
| **Endpoint** | `https://app.nextupwalkin.com/mcp` |
| **Transport** | Streamable HTTP |
| **Authentication** | None. No sign-in, no API key. |
| **Access** | Read-only. Nothing is written on a person's behalf. |
| **Registry name** | `com.nextupwalkin/nextup-walk-in` ([Official MCP Registry](https://registry.modelcontextprotocol.io)) |

## Connect

Add the endpoint as a remote (custom) MCP server in any client that supports Streamable HTTP.

**Claude Code**

```bash
claude mcp add --transport http nextup-walk-in https://app.nextupwalkin.com/mcp
```

**Clients configured with JSON** (such as Cursor or VS Code)

```json
{
  "mcpServers": {
    "nextup-walk-in": {
      "url": "https://app.nextupwalkin.com/mcp"
    }
  }
}
```

## Tools

| Tool | What it does |
| --- | --- |
| `find_walk_in_places` | Finds businesses that take walk-ins, by US city and state, business name or ZIP code, with the current wait. It can narrow results to one kind of business. Open places come first, shortest wait first. |
| `get_place` | Gives one place's details: address, today's hours, whether its line is taking check-ins, the current wait, its team members and the link to join the line. |
| `get_services` | Lists the services a place offers publicly, with the prices and lengths it shows customers. |
| `available_times` | Lists open start times on one date for one service, at places that take online bookings. |
| `join_link` | Gives a link the person taps to join the line, with a service and team member filled in if given. |
| `booking_link` | Gives a link the person taps to book an appointment at a set time. |
| `purchase_link` | Lists a place's packages and memberships sold online, with the link to buy them. Only for places whose owner turned it on. |

## How it behaves

- **The person always confirms.** The server never joins a line, books or pays for anyone.
  It returns links, and the person confirms on the business's own page. Payments happen on
  the business's own secure Stripe page.
- **Waits are estimates.** They're read live but can change at any moment, so assistants
  should say "about." When a place doesn't estimate minutes, the server returns the number
  of people waiting instead.
- **Public information only.** The server returns only what a business already shows
  customers on its public NextUp Walk-in page. It never returns customers' personal details.
- **Requests are rate-limited** per client to keep the service fair for everyone.

## Example questions

- "Who has the shortest walk-in wait for a haircut in Austin, TX right now?"
- "How long is the walk-in wait at the barbershop on Main Street?"
- "Is there a nail salon taking walk-ins in ZIP 80302?"
- "Find me an open time for a beard trim on Friday."

## Support

- Email: support@nextupwalkin.com
- Website: https://nextupwalkin.com
- Security issues: see [SECURITY.md](SECURITY.md)

## License

Copyright © 2026 NextUp LLC. All rights reserved. This repository is documentation only. You
may connect to and use the hosted server under the
[NextUp Walk-in terms](https://nextupwalkin.com/terms).
