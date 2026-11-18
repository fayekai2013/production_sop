# Paid Service Authorization

## Classify the route before any generation or call

- List every route the task could use before starting work and mark each one as built in and free to use, prepaid under an existing plan, or charged per use.

- Treat any service that bills by call, token, second, render or credit as pay per use even when a credential is already connected and even when the expected amount is small.

- Check whether the built in route actually works before planning around a paid fallback.

- Stop planning the paid route the moment a built in route is blocked and carry the blocker into the approval request instead of quietly switching routes.

## Get explicit approval for this project

- Ask for approval that names the service, the project, what will be bought and an estimated cost range before the first paid call.

- Count only the user's own approval of that named service and cost as authorization.

- Never count a do not ask instruction, an instruction to work independently, a connected account, an available API key, a budget you set yourself or a stop line you set yourself as authorization.

- Never count telling the user in a progress update after the fact as authorization.

- Present the blocked built in route, the paid alternative and its cost together so the user decides with the trade off in view.

## Act only inside the approved scope

- Spend only on the service, project and work the approval named and stay inside the approved cost range.

- Stop before exceeding the approved range, switching to a different paid service or reusing an approval from an earlier project and ask again.

- Keep a running total of paid calls as you work with the service, the time, the item produced and the amount.

- Pause the paid work and report as soon as the running total approaches the approved range rather than finishing first and explaining later.

## Report the spend against the approval

- Report the final amount beside the approved range and the service and project it was spent on at delivery.

- Name any paid call that happened without approval plainly as unauthorized and state the amount without framing it as a budget overrun.

- Record the approval and the final total in the project record so a later session can check the spend without rereading the whole conversation.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Alternatives for better results

- Finish every free part of the work first so the approval request covers a real remaining gap rather than a hypothetical one.

- Offer a free local route such as assembling still images into a finished file when one exists and state honestly where its quality differs.

- Run one small paid pilot only after approval so the user can judge the result before the full amount is committed.
