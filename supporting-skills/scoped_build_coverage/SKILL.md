# Scoped Build Coverage and Product First Delivery

## Freeze the promised scope into a ledger

- Write the promised scope as a ledger before work starts, with one row for each promised domain, source, deliverable section and final product.

- Give each row a state of covered, partial, absent or undecided, and keep the count of rows in each state current as work proceeds.

- Treat the final usable product as a ledger row of its own, separate from its raw material, indexes and supporting files.

## Audit the plan itself for gaps

- Check the original plan against the field before deep work starts, and add obvious missing sources, domains or methods to the ledger as absent rows instead of leaving them outside the count.

- Recheck the ledger against the plan whenever a phase closes, because a phase can report complete while the wider promised scope is still mostly absent.

- Mark anything not yet decided as undecided in the ledger, and never present undecided or partial rows as settled findings.

## Build the usable product before governance expands

- Assemble a first usable version of the product the user asked for as soon as its core material exists, before spending further time on codebook governance, backfill, scoring refinements or corpus accounting.

- Freeze governance and backfill work once it is good enough to support the product, and reopen it only when the user asks or a named defect blocks use of the product.

- Report an empty or unusable primary deliverable as a blocker in the next progress update, not as a detail the user has to discover by asking.

## Report coverage in every progress update

- State coverage in every progress report as counts from the ledger, such as domains covered out of domains promised and sources completed out of sources inventoried.

- Name the absent and partial rows in the same report, so the user never has to ask whether everything promised was actually found.

- Describe progress in terms of the product and its coverage, not only in internal accounting terms the user did not ask for.

## Gate final assembly on coverage

- Run a gap audit against the ledger before final assembly, and complete or explicitly defer every absent row before claiming the build is complete.

- Have an independent reviewer recount the finished product against the ledger, including the total item count and the index, and pass the build only when the recount matches.

- Deliver with the final coverage counts and any deferred or undecided rows named plainly, so the closing state is honest even when it is not perfect.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.

## Alternatives for better results

- Narrow the promised scope openly with the user when the full scope cannot be covered, rather than quietly covering a fraction and reporting activity.

- Pilot one domain end to end through to the assembled product before scaling the build, so the product shape is proven before governance grows around it.
