# VeyroHood Verify

Live: https://burhanu6720.github.io/veyrohood-whitelist/

## Files
- `index.html` — wallet connect, missions, $0.25 pay, auto approve
- `art.svg` — live animation (upload `art.gif` in the same folder if you have a custom GIF)

## Supabase SQL (run once)

```sql
alter table public.submissions add column if not exists tx_hash text;
alter table public.submissions add column if not exists referrer text;

alter table public.submissions alter column role drop not null;
alter table public.submissions alter column role set default 'RANDOM';

create unique index if not exists submissions_wallet_unique on public.submissions (lower(wallet));
create unique index if not exists submissions_tx_unique on public.submissions (tx_hash);

alter table public.submissions enable row level security;

drop policy if exists "Allow public insert" on public.submissions;
drop policy if exists "Allow public read approved" on public.submissions;

create policy "Allow public insert"
on public.submissions for insert
to anon, authenticated
with check (true);

create policy "Allow public read approved"
on public.submissions for select
to anon, authenticated
using (status = 'approved');
```

## Notes
- Payment goes on-chain to `0xf6F80827cBAf83798c7763FCd915C0068F2bE60C` on Robinhood Chain (4663).
- First 1000 approved wallets = OG, rest = RANDOM.
- Referral 20% is displayed only. Withdraw is manual until a contract is added.
