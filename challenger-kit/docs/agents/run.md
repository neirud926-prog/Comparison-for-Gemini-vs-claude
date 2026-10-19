# The Run

How to build this app from Spec #1 (issue #1) to a reviewed app. Work the steps in order; each ends on its **done when** line. Domain words are defined in `GLOSSARY.md`, decisions of record are in `docs/adr/`, and the spec's three test seams (label core, data folder, render round-trip) set where tests go.

The human answers your questions and is watching from a phone. Put every decision that is theirs to them: a product rule the spec leaves open, or a trade-off between two valid designs. Finding facts is your job.

## 1. Tickets

Run `/to-tickets` on issue #1.

**Done when** every user story and implementation decision in #1 belongs to a ticket, each ticket is a sub-issue of #1 labelled `ready-for-agent`, and each ticket's blockers are recorded.

## 2. Build, ticket by ticket

Repeat until every child of #1 is closed:

1. Pick the frontier ticket: open, every blocker closed, first in #1's order.
2. Run `/implement #<n>`. It drives `tdd` at the spec's seams, then `code-review`, then commits.
3. Push to `main`.

**Done when** the full test suite and typecheck pass, CI is green on that push (once the skeleton ticket has added CI), and the ticket is closed with a comment listing its commits. A red CI is the next thing to fix, ahead of the next ticket.

Work that needs the Pharmacy PC, the GX420t printer or the UROVO DT50S scanner belongs to the human's UAT. Write it into a UAT checklist in the README and keep going.

## 3. Review round

1. Close #1. Run `/ponytail-review` over the whole app.
2. Turn the findings worth fixing into one spec with `/to-spec`, then into tickets with `/to-tickets`.
3. Build those tickets exactly as in step 2.

**Done when** every review ticket is closed, CI is green on `main`, and you have told the human "Run complete" with the list of closed issues.
