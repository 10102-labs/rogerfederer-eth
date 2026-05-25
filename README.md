# rogerfederer.eth

A decentralized tribute site exploring Roger Federer's disruption of tennis — and the parallel to Bitcoin's disruption of finance.

Built by [10102](https://10102.io) · permanence and self-custody on Ethereum.

Live at: [rogerfederer.eth.limo](https://rogerfederer.eth.limo)

## Stack

Pure HTML / CSS / JS. No build step, no framework, no bundler. Designed to outlast JavaScript trends and survive on IPFS as a permanent artifact.

```
public/
├── index.html
├── style.css
└── script.js
```

`.github/workflows/omnipin.yml` deploys `./public` to IPFS via [Omnipin](https://github.com/omnipin/omnipin) and updates the ENS contenthash on `rogerfederer.eth`.

## Deploy

See [DEPLOYMENT.md](./DEPLOYMENT.md). TL;DR: push to `main` after configuring the required secrets and variables. The workflow pins the `public/` folder to Pinata + Lighthouse + 4everland and updates the ENS record.

## Local preview

```bash
cd public && python3 -m http.server 8000
# open http://localhost:8000
```
