# PrizeVault — hackathon prizes that pay themselves, on GenLayer

Hackathon winners often wait months for prizes, and some never get paid. Organizers have announced big pools, then never published winners at all.

**PrizeVault locks the whole prize pool before the hackathon starts and pays winners straight from the official results page.**

## Flow
1. **Organizer creates a vault.** The value sent is the pool. It sets a fixed split (`split_bp`, e.g. `5000,3000,2000`), the official results page URL, the registration period, the deadline to publish results, and the claim window.
2. **Teams register.** Each wallet registers its project name and project page once. A project URL can't be registered twice.
3. **`check_results` (anyone).** Validators read the results page and decide whether final winners have been published (not "judging in progress", not just finalists).
4. **`claim_prize(place)`.** Validators confirm the claimer's project is listed at that place. In live mode, plain string matching (not AI) also confirms the claimer's wallet appears on its own project page, so impostors can't claim.
5. **No results by the deadline + grace (3+ checks).** The vault goes to `NO_RESULTS`. Each team proves with `verify_entry` that its page is a real submission to this hackathon. After the window, every verified team takes an equal share with `claim_share`.
6. **`close`.** Unclaimed prizes return to the organizer only when results were published honestly. In `NO_RESULTS` the organizer gets money back only if no team verified.

## Safety design
- AI answers narrow yes/no questions (published? this project at this place? real submission?), and validators must match (`strict_eq`).
- The wallet-on-page check is deterministic code.
- Each vault holds only its own pool. No shared pool. Prize amounts are fixed at creation, and rounding dust goes to 1st place.
- Each place is paid once, each team wins at most one place, and shares are paid once.
- Registration closes on time, and in live mode the organizer can't register a team.
- There is a retry gap between results checks. Demo and live are separate deployments (minutes vs days).
- Page text is treated as data, never as commands.

| Window | Demo | Live |
|---|---|---|
| Time unit | minutes | days |
| Grace after results deadline | 2 min | 14 days |
| Retry gap between checks | 30 s | 6 h |
| Wallet must be on project page | no (Studio accounts) | yes |

## Demo pages (`/demo`)
| File | Use |
|---|---|
| `results-published.html` | winners out (TrustLink 1st, PayLoop 2nd, ProofKit 3rd) |
| `results-pending.html` | judging in progress → no results |
| `project-trustlink.html` | real submission page |
| `project-empty.html` | placeholder page → not real |

## API
`create_vault(name, results_url, split_bp, register_for, results_within, claim_window)` payable ·
`register(vault_id, project_name, project_url)` · `check_results(vault_id)` · `claim_prize(vault_id, place)` ·
`verify_entry(vault_id)` · `claim_share(vault_id)` · `close(vault_id)` · views: `get_vault`, `get_entry`, `get_count`, `get_info`

Built by Sankar D (gurusankar55) for the GenLayer Builders Portal.
