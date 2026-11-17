# Events API: the read-only parts

Read this before the first API call and whenever a call fails.

The AWS Events API lists AWS events such as re:Invent, serves an event's session catalog, and manages one attendee's own schedule. The awsevents MCP server offers it as tools. It acts only for the person signed in on that machine. The API is new, so when this file and the API disagree, the live OpenAPI description at https://api.awsevents.com/v1/openapi.json is right. The guide is at https://docs.aws.amazon.com/events/latest/devguide/what-is-events-api.html.

## Tools this skill uses

| Tool | Use |
|---|---|
| `GetEvent` | The event's name, dates and timezone. Works after the event ends |
| `GetSchedule` | The attendee's `reserved` and `favorites` (session IDs), `personalTime`, and `invitedSessions` (whole sessions) |
| `GetSession` | One session by its `sessionId`: title, code, abstract, speakers, time, room, services. It does not accept the code |
| `ListSessions` | Not used to find a session. It cannot search or filter, a page holds 250 sessions, and the catalog has thousands, so never walk it |

Some clients prefix the tool names. Kiro shows `AWSEventsPublicApi-Mcp-prod___GetEvent`, so match on the operation name at the end.

Never call `ReserveSessions`, `CancelReservation`, `AssociateFavorites`, `DisassociateFavorite`, `CreatePersonalTime`, `UpdatePersonalTime` or `DeletePersonalTime`. A trip report must not change anyone's schedule. `ListEvents` is only for finding an event ID you do not know, and it hides events that have ended.

## Set up the server

Every MCP call needs a sign-in, including catalog reads. Reading an event that requires registration, such as re:Invent, also needs the attendee to be registered for it. The sign-in uses an AWS Builder ID, the free personal AWS login that is used to register for re:Invent. The client opens a browser on the first tool call and keeps the token on the attendee's machine.

The developer guide documents working settings for two clients. Show the settings and ask before editing the attendee's MCP configuration. Keep their other entries.

**Kiro.** Add this to the MCP configuration file, then reload the window.

```json
{
  "mcpServers": {
    "awsevents": {
      "url": "https://api.awsevents.com/mcp",
      "oauth": {
        "clientId": "7vmom55m1qstvq8i71ph127bfq",
        "redirectUri": "http://127.0.0.1:8976",
        "oauthScopes": ["openid", "email", "events/access"]
      }
    }
  }
}
```

End `redirectUri` at the port. Kiro adds the `/oauth/callback` path itself, and a path or a different port makes sign-in fail with `redirect_mismatch`.

**Claude Code.**

```
claude mcp add --transport http --scope user awsevents \
  https://api.awsevents.com/mcp \
  --callback-port 8484 --client-id 7vmom55m1qstvq8i71ph127bfq
```

**Other clients.** The endpoint is `https://api.awsevents.com/mcp` (streamable HTTP) and the shared public client ID is `7vmom55m1qstvq8i71ph127bfq`. The guide lists no callback for other clients, and the API matches the callback exactly. If sign-in does not work, go on without the API.

To confirm the connection, call `GetEvent` with `reinvent2026`.

## Schedule and session shapes

`GetSchedule` returns `{"schedule": {...}}`:

- `reserved`: session IDs the attendee holds a seat in. A session with `isReservable: false` never appears here.
- `favorites`: session IDs. A favorite is interest only, not a seat. For sessions that cannot be reserved, it is the only way onto a schedule.
- `personalTime`: entries with title, description, location, start and end in the event's local time.
- `invitedSessions`: whole session objects the event placed on the schedule. They may not be in the catalog, so `GetSession` may not find them. They need no lookup.

Nothing in the schedule says what the attendee attended.

`GetSession` returns `{"session": {...}}`. Only `sessionId` and `title` are guaranteed. Everything else may be missing: `abbreviation` (the session code), `abstract`, `type`, `level`, `room`, `venue`, `isReservable`, `seatAvailability`, `sessionTime` (`date`, local `time` and `length` in minutes as a string, and sometimes `timezone`), `speakers`, and the tag lists `topics`, `services`, `areasOfInterest`, `roles`, `industries`, `features`, `experiences` and others.

- Read every field defensively and leave out what is absent.
- When `sessionTime` has no `timezone`, use the event's `timezone` from `GetEvent`. For re:Invent 2026 that is America/Los_Angeles.
- Do not branch on the exact wording of `type` or `level`. On 17 November 2026 the catalog used these types: Breakout session, Chalk talk, Workshop, Builders' session, Lightning talk, Code talk, Lab, Bootcamp, Gamified learning, Exam prep, Keynote, Mainstage, Meetup, Expo, Featured experience and Event service. Levels read like "200 – Intermediate", with an en dash, and some sessions have none.
- Breakout sessions, lightning talks, keynotes, mainstage events, meetups, expo, featured experiences and event services all have `isReservable: false`. Chalk talks, code talks, workshops, builders' sessions, bootcamps, exam prep, gamified learning and most labs can be reserved.
- A code ending in `-R` is a repeat showing. The catalog lists such a code without a matching code that lacks the suffix, so treat a code with and without `-R` as the same session.
- A speaker is one string that includes title and company, for example "Diego Porta, AWS".
- Show times in the event's local time with the timezone name. Use the `timezone` value, not an abbreviation.

## Errors

| Status | Meaning | What to do |
|---|---|---|
| 401 | No valid sign-in | Tell the attendee a browser sign-in is needed. Retry once after it |
| 403 with a JSON body | Signed in, but not registered for this event | Stop and do not retry. The usual cause is signing in with a Builder ID other than the one on the registration |
| 403 with no body | The edge refused requests from this address | Slow down and retry later |
| 404 | The event or session does not exist | For a session code, ask the attendee for the title |
| 409 | The operation is closed for now | Not expected for reads |
| 429 | Per-minute quota used | Wait the number of seconds in `Retry-After` |
| 500 or 503 | Service trouble | Retry the read with backoff, then go on without the API |

Quotas per attendee per minute: `GetSchedule` 60, `GetSession` 120, `ListSessions` 120. A report of 30 sessions uses about 30 `GetSession` calls.

## Signing out

Signing in leaves a token on the attendee's machine for up to 30 days, and deleting the server's configuration does not revoke it. The steps to sign out fully are in https://docs.aws.amazon.com/events/latest/devguide/auth-signing-out.html. The attendee can also end their Builder ID browser session at https://profile.aws.amazon.com.
