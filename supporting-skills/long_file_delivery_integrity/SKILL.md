# Long File Delivery Integrity

## Plan the intended file before writing

- Write down the intended structure before the first write, as a list of the sections in order, the planned final line and a plausible size range for the finished file.

- Treat a rewrite, a language conversion and a regeneration as a new delivery, not as a minor edit of the old file.

- Keep the intended structure beside the work so the later check compares the file against intent, not against memory of what was written.

## Write in chunks

- Write a long file in parts, with a first write followed by appends or edits that each carry one section or one small group of sections.

- Never deliver a long file from a single giant write when the content can be split at section boundaries.

- Read back each part after writing it and confirm the part ends where it was meant to end before adding the next part.

- Stop and repair a part as soon as a read back shows missing or cut off content, rather than writing the remaining parts on top of a broken base.

## Check completeness against intent

- Check the finished file against the intended structure, section by section, and confirm every planned section is present in order.

- Confirm the file size falls inside the planned range and treat a file far below that range as incomplete until the missing content is found.

- Confirm the file ends on the planned final line.

- Search the full text for truncation markers and unfinished work markers such as the word truncated, TODO and placeholder text, and treat any hit in delivered content as a failed check.

- Read the delivered file itself at its head, its middle and its tail before reporting its state.

- Never treat agreement between a copy and the file it was copied from as a completeness check, because a broken original and its copy agree exactly.

- Never diagnose a reported problem as a preview or display issue before reading the delivered file itself.

## Report only what the check showed

- Claim a long file is complete only after every check above has passed on the delivered file itself.

- Report the section list found, the file size and the final line when you report a long file as delivered, so the claim can be checked.

- Report a failed check plainly, name the missing or damaged part, repair the file and run the full check again before claiming completion.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Alternatives for better results

- Split the work at natural boundaries such as a chapter or a major section when a file is long enough that even chunked writing risks losing a middle part.

- Keep a short independent checklist file for very long deliveries and tick each section off as its read back passes, instead of holding the structure in working memory.

- Deliver in stages the user can open and confirm when a file is both long and high stakes, rather than presenting one finished claim at the end.
