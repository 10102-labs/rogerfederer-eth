# Deployment

This site auto-deploys to IPFS and updates the `rogerfederer.eth` ENS contenthash on every push to `main`, via [Omnipin](https://github.com/omnipin/omnipin). The workflow is at `.github/workflows/omnipin.yml`.

## One-time setup

### 1. Generate a fresh deploy key

Do **not** reuse your main wallet — generate a wallet that only ever does ENS contenthash updates for this site.

```bash
cast wallet new
# or use any wallet tool; save the private key
```

Fund the address with a small amount of ETH (~$5-15) for contenthash update gas. Each push to `main` triggers a transaction.

### 2. Get free pinning provider tokens

- **Pinata** — pinata.cloud
- **Lighthouse** — lighthouse.storage
- **4everland** — 4everland.org

Free tiers on each are sufficient. Omnipin pins to all three for redundancy.

### 3. Configure the GitHub repo

Settings → Secrets and variables → Actions.

**Variables** (visible to workflows, not encrypted):

| Name | Value |
|---|---|
| `OMNIPIN_ENS` | `rogerfederer.eth` |
| `OMNIPIN_SAFE` | (optional) `0x...` of a Safe multisig if using one |

**Secrets** (encrypted):

| Name | Value |
|---|---|
| `OMNIPIN_PK` | the fresh private key with `0x` prefix |
| `OMNIPIN_PINATA_TOKEN` | Pinata API key (with `pinFileToIPFS` permission) |
| `OMNIPIN_LIGHTHOUSE_TOKEN` | Lighthouse API key |
| `OMNIPIN_4EVERLAND_TOKEN` | 4everland API key |

### 4. Trigger first deploy

Push any change to `main`, or use the "Run workflow" button on the Actions tab (workflow_dispatch).

The workflow:
1. Installs Omnipin via bun
2. Pins `./public/` to Pinata, Lighthouse, and 4everland
3. Calls `setContenthash` on `rogerfederer.eth`'s ENS resolver (or proposes a Safe transaction if `OMNIPIN_SAFE` is set)

## Verify after deploy

- `https://rogerfederer.eth.limo`
- `https://rogerfederer.eth.link`
- Direct IPFS: `https://ipfs.io/ipfs/<CID>` (CID is in the workflow logs)

Allow 5-15 minutes for ENS resolver propagation after the contenthash transaction confirms.

## Notes

- The deploy job is gated on `OMNIPIN_ENS != ''` — a fresh clone or fork won't fail builds until you configure the variable.
- Pure HTML site → no build step → IPFS receives exactly what's in `public/`.
- `README.md` and `DEPLOYMENT.md` live at repo root, **not** in `public/`, so they aren't deployed to IPFS.
