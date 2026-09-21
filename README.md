# QC Phi

Quality-control observer for the Phi network.

QC Phi does **not** repair production applications itself. It observes expected contracts, records failures, and emits repair tickets that can be handed to Code Phi or another coding agent.

See [OBSERVER-REACTOR-MAP.md](OBSERVER-REACTOR-MAP.md) for the current system owners, dependency map, contract inventory, safety rules, and ordered repair backlog.

## First contracts

- Infinity Phi successful search -> token ledger transaction -> wallet tally refresh
- Collect -> destination receives the same selected card/media
- AI Overview handoff preserves selected image/video/audio collections
- Website Index receives collected cards
- Share produces valid social-preview metadata
- News Phi and Builder Reserve feeds remain connected
- Navigation endpoints do not return 404
- Previously passing contracts are rechecked after changes

Open `index.html` to run browser-side checks and copy an AI repair ticket.

Every failure records the system, contract, expected behavior, observed behavior, evidence, severity, timestamp, and suggested inspection target. A repair agent should verify the failure before changing code and rerun the contract afterward.
