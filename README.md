# Presentation Timer

A single-page countdown timer for talks: presentation time, a one-minute
"wrap up" warning, then Q&A. No build step and no dependencies — everything
lives in `public/index.html`.

## Run locally

Open `public/index.html` in a browser.

## Deploy to Cloudflare Pages

Deploys use [Wrangler](https://developers.cloudflare.com/workers/wrangler/),
Cloudflare's CLI. `wrangler.toml` already points it at the `public/` folder.

### Update the existing site

```sh
npx wrangler login                        # once per machine; opens the browser
npx wrangler pages deploy --branch main   # publish public/ to production
```

`--branch main` makes it a production deploy. Any other branch name creates a
preview deploy at `https://<branch>.presentation-timer-6v2.pages.dev`.

### Set up from scratch (new Cloudflare account)

Create the Pages project once, then deploy as above:

```sh
npx wrangler login
npx wrangler pages project create presentation-timer --production-branch main --force
npx wrangler pages deploy --branch main
```

`--force` is needed only on `project create`. Without it, recent Wrangler
versions try to turn the project into a Workers project, and that fails for
this setup. If the name `presentation-timer` is already taken, Cloudflare adds a
suffix to the `*.pages.dev` address. The command output shows the final URL.

### Alternative: deploy on every git push

In the Cloudflare dashboard, go to **Workers & Pages → Create → Pages →
Connect to Git**, pick this repository, and set:

| Setting                | Value     |
| ---------------------- | --------- |
| Production branch      | `main`    |
| Build command          | *(empty)* |
| Build output directory | `public`  |

After that, every push to `main` deploys automatically.

Note: Cloudflare can't switch an existing CLI-created project to Git deploys. To
use this option, create a new Pages project in the dashboard (or delete the
current one first).

### Custom domain

In the dashboard, open **Workers & Pages → presentation-timer → Custom domains**
and add your domain.
