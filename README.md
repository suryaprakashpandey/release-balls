# Release Balls

**Race the people who shipped your release.** Paste a GitHub repository and every contributor becomes a ball with their avatar. The more pull requests they merged, the faster their ball rolls. Download the race as an MP4 or GIF and drop it into your release notes, changelog or social post.

**Try it:** https://release-balls.vercel.app

Release Balls runs entirely in your browser. There is no backend, no sign-up and no build step: the whole app is one `index.html`.

## How to use it

1. Open the site and connect GitHub. This is a one-time step: click **Generate token on GitHub**, create a read-only token, and paste it in. It's saved in a cookie in your browser for 90 days.
2. Paste a repository, like `https://github.com/pingdotgg/t3code` or just `pingdotgg/t3code`.
3. Choose what to rank and click **Start the race**.
4. Download the result as MP4 (1080px, 60fps), GIF (480px, 15fps) or JSON.

### What you can rank

| Mode | What it counts |
| --- | --- |
| **Time window** | Every PR merged in the past 24 hours, week, 2 weeks, 30 days, or since a date like `2026-09-01`. |
| **Releases** | PRs linked in the notes of the latest N releases. Filter tags by text, like `nightly` or `desktop`. |
| **One tag** | PRs linked in the notes of one release, like `v1.2.0`. |

Score contributors by **PRs authored**, **PRs merged**, or **lines changed**. Show the top N (or `0` for everyone), or limit the race to specific users.

### Good to know

- **Release modes count PRs linked by full URL** in the release notes, such as `https://github.com/owner/repo/pull/123`. GitHub's auto-generated release notes include these. Releases whose notes link no PRs are skipped, and the rankings say which ones.
- **Bots are left out.** Ties sort alphabetically.
- **Time windows are capped at 1,000 PRs** because GitHub search returns no more than that. Pick a shorter window for very busy repos.
- **Races last 16 seconds.** The fastest ball crosses the track in one second, so speeds are relative within a race.

## Your GitHub token

A token is required because the site reads releases and pull requests through the GitHub API. Here's how it's handled:

- It's stored only in a cookie in your browser, for 90 days. Use **Remove token** in the top bar to delete it.
- It's only ever sent to `api.github.com`. There is no Release Balls server.
- Create a **fine-grained token with read-only access**. Public repos need no extra permissions. For private repos, select them and grant **Contents: read** and **Pull requests: read**.
- The page's own code reads the cookie to call GitHub, so it isn't an HttpOnly cookie. Use a read-only token with an expiry date.

With a token GitHub allows 5,000 API requests an hour, which covers hundreds of races.

## Automate it with GitHub Actions

The site generates a workflow file from your settings (see **Automate with GitHub Actions** on the page). It uses the original [maria-rcks/release-balls](https://github.com/maria-rcks/release-balls) action to render the race on every release, or on a schedule, and attach it to your release notes.

## Run it locally

No install needed. Serve the folder with any static file server:

```sh
git clone https://github.com/suryaprakashpandey/release-balls.git
cd release-balls
python3 -m http.server 8000
```

Then open http://localhost:8000.

Video export uses the browser's WebCodecs API (recent Chrome, Edge and Safari). Other browsers fall back to recording the canvas in real time.

## How it works

- **Data:** the GitHub REST API for releases and search, and the GraphQL API to read PRs in batches of 50.
- **Rendering:** a 1080×1080 canvas, using the same layout and ping-pong motion as the original action.
- **Export:** [mp4-muxer](https://github.com/Vanilagy/mp4-muxer) with WebCodecs for MP4, and [gifenc](https://github.com/mattdesl/gifenc) for GIF, both loaded from jsDelivr.

## Credits

Release Balls is a browser version of the [Release Balls GitHub Action](https://github.com/maria-rcks/release-balls) by maria-rcks. The ranking rules, layout and motion are ported from its Rust source. Not affiliated with GitHub.

## Contributing

Issues and pull requests are welcome. Everything lives in `index.html`, so a change is usually a single-file diff. Test it with a local server as shown above.

## License

[MIT](LICENSE)
