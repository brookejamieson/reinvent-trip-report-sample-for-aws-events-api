# Verifying launches

Read this at step 8 of the workflow.

Attendees remember launches by nickname, by the wrong status or by the wrong week. A trip report that names a service wrongly, or calls a preview "available", costs the author credibility with the people they send it to. Check each service or feature the attendee names before it goes in the report.

## Tools

Use the AWS MCP Server's documentation tools, from the Agent Toolkit for AWS: `aws___search_documentation` to search and `aws___read_documentation` to read a page. No AWS account is needed for these. AWS allows unauthenticated requests to its read-only documentation tools at a lower rate limit (5 requests per second per source IP address). If the attendee's agent does not have these tools, say the launches were not verified, list them as unverified in chat, and suggest the attendee check https://aws.amazon.com/about-aws/whats-new/.

## How to search

- Search for the feature by name, as the attendee said it, plus what it does. Set `topics` to `["current_awareness"]`, the topic for new launches and announcements. Example: `search_phrase` "Amazon Bedrock AgentCore Evaluations generally available".
- Do not use generic queries such as "re:Invent 2026 new launches". They return unrelated results.
- Under topic `general`, the same searches tend to return reference pages and blog posts and miss the dated What's New post. Use `current_awareness` first.
- If the first search misses, try the parent service plus the feature, or drop adjectives the attendee added.
- Read the top result. On a What's New post the launch date is the "Posted on" date, exactly as the page shows it (for example "Posted on: Nov 11, 2026"). Do not convert timestamps between timezones. On a blog post, an "Updated on" note changes the status only when it says so (for example "Updated on March 31, 2026 – Amazon Bedrock AgentCore Evaluations is now generally available"). A note that adds links or fixes a typo is not a launch date.
- During the week, AWS may publish a roundup on the AWS News Blog called "Top announcements of AWS re:Invent 2026", as it has in earlier years. Search for it by that name and use it to cross-check names. Do not assume it exists.

## What to write

- Use AWS's name for the launch, not the attendee's nickname.
- Give the status as of today (preview, generally available, which Regions) and the date from the post, with a link.
- Do not call something a re:Invent launch unless the post's date and text say so. A post dated before the event was not announced at it. Say what the post says.
- If the attendee's description is out of date (for example "still in preview" and the post says generally available), use the post's status and tell the attendee what you changed.
- If you find nothing, keep the attendee's wording and list the item as unverified in chat. Never invent a name. Write no caveat about verification into the report: "I have not verified this" is something the attendee never said, so it belongs in chat.
- If the attendee names a Region, `aws___get_regional_availability` can check availability. It needs the exact product name and may not know a brand-new launch.
