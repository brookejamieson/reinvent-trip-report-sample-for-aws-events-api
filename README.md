# re:Invent trip report skill

A skill for coding agents such as Kiro and Claude Code that writes your AWS re:Invent trip report from your own notes, so you can spend your evenings with the people you're hanging out with instead of writing it in your hotel room. Built for re:Invent 2026, 30 November to 4 December in Las Vegas.

Session titles, abstracts, speakers and times come from the AWS Events API, so you don't retype them. The skill asks you what you thought, and leaves out any section you have nothing to say about.

This sample works with the AWS Events API.

## What it does

Ask your agent for your trip report and the skill:

1. Reads your reservations and favourites from the Events API.
2. Lists those sessions and asks which ones you went to. Breakout sessions, lightning talks and keynotes can't be reserved, and a reserved seat is released if you arrive late, so the list is only a starting point.
3. Asks two or three questions about each session you pick, then a few about the week: the themes you noticed, the launches that matter to you, the people you met, and what you'll do first back at work.
4. Checks each new service or feature you mention against AWS docs, so the report uses AWS's name for it, its date, and whether it's in preview or generally available.
5. Writes the report in the first person, aiming for 500 to 700 words unless you ask for another length, and saves it as `trip-report.md` in your working folder. If your notes are thin, the report is shorter instead of padded.

A session that isn't on your schedule needs its title from you, and its code too if you have it, such as `AIM2216`. The API looks sessions up by a long `sessionId` and can't search by code.

### Keep notes during the conference

At the end of a conference day, say "log my day". The skill lists that day's sessions, asks which you went to, then asks what stood out, what surprised you and what you'll try. It appends your answers, in your words, to `trip-notes.md`. When you ask for the report, it reads those notes first and asks only for what's missing.

The Events API guide doesn't say whether your schedule stays readable after the event ends. The notes record each day's sessions by code and title, so you can still get a report if it doesn't.

## What you need

To read your schedule you need to be registered for re:Invent 2026 and signed in with the AWS Builder ID from your registration. A Builder ID is the free personal sign-in AWS uses for its developer tools and events. AWS documents that sign-in for Kiro and Claude Code. With another agent, use the skill without the API (see [Without the API](#without-the-api)).

## Install

Copy the `reinvent-trip-report` folder into your agent's skills folder.

| Agent | Global | Project |
|---|---|---|
| Kiro | `~/.kiro/skills/` | `.kiro/skills/` |
| Claude Code | `~/.claude/skills/` | `.claude/skills/` |
| Codex | `~/.agents/skills/` | `.agents/skills/` |
| Cursor | `~/.cursor/skills/` | `.cursor/skills/` |
| fx | `~/.fx/skills/` | `.fx/skills/` |

## Connect the Events API

The Events API has an MCP server at `https://api.awsevents.com/mcp`, which lets your agent call it as a set of tools. Every call needs a sign-in, even reading the catalog, so your client prompts you to sign in with the Builder ID from your registration, usually by opening a browser. A Builder ID that isn't registered for re:Invent 2026 gets a 403, and signing in again with it doesn't help.

### Kiro

Add this to your MCP configuration and reload the window.

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

### Claude Code

```
claude mcp add --transport http --scope user awsevents \
  https://api.awsevents.com/mcp \
  --callback-port 8484 --client-id 7vmom55m1qstvq8i71ph127bfq
```

If the skill can't find the server, it shows these settings and asks before it changes your configuration. To check the connection, ask your agent to call `GetEvent` for `reinvent2026`.

## Use it

Start with a prompt like one of these:

- "Write my re:Invent trip report for my team"
- "Log my day"
- "I also went to Trust in Depth: Giving agents more freedom without losing control (AIM2216). Add it."

## Without the API

If you can't or won't sign in to the Events API, give the skill the title of each session you want in the report, and its code if you have it. You can copy both from the AWS Events app. Without the API the skill has no abstracts, speakers or times, and it doesn't make them up, so the report holds only what you give it.

## Checking launches

Launch checks use the documentation tools in the AWS MCP Server, part of the [Agent Toolkit for AWS](https://github.com/aws/agent-toolkit-for-aws). AWS's [quotas page](https://docs.aws.amazon.com/agent-toolkit/latest/userguide/aws-mcp-limits.html) says those read-only tools accept unauthenticated requests at a lower rate limit, and the [setup guide](https://docs.aws.amazon.com/agent-toolkit/latest/userguide/getting-started-aws-mcp-server.html) covers connecting the server with OAuth or SigV4. If your agent doesn't have the tools, the skill tells you the launches weren't verified.

## What it won't do

The skill only reads from the Events API. It never changes your schedule, and it leaves your personal time out of the report unless you ask. Your MCP client handles sign-in and holds the token. The skill tells the agent never to read, store or print it.

## Signing out

Signing in leaves a token on your machine for up to 30 days, and deleting the server's configuration doesn't revoke it. The full sign-out steps are in the [Events API guide](https://docs.aws.amazon.com/events/latest/devguide/auth-signing-out.html). You can also end your Builder ID browser session at [profile.aws.amazon.com](https://profile.aws.amazon.com).

## Files

`reinvent-trip-report/SKILL.md` holds the instructions the agent follows. The files in `references/` next to it are read only when the agent needs them. It's all Markdown, with no scripts to run.
