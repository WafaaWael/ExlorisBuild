# Exloris Web Build

## First deployment

1. Commit and push the repository using GitHub Desktop (not the website uploader).
2. In GitHub, open Settings → Pages → Build and deployment → Source and select **GitHub Actions**.
3. Open Actions → Deploy Exloris to GitHub Pages → Run workflow if the first push ran before Pages was configured.
4. Open the website URL shown by the successful deployment.

The workflow publishes the game at the site root, not `/Build/`.

## After exporting a new Unity build

Export into `Build/`, using the same Brotli build filenames as this export. Then run:

```sh
node scripts/pages-build.mjs split
```

Commit the updated build, `data-parts` chunks and manifest, then push. Node.js 22 or newer is recommended.
Do not commit the original `Build/Build/Build.data.br`: it is deliberately ignored and preserved locally.
The splitting command regenerates its own chunk files, removing obsolete generated chunks only.

GitHub Actions verifies SHA-256 checksums, reassembles the data, decompresses the three Unity assets,
and updates their URLs in the deployed HTML and service worker. GitHub Pages cannot configure
the Brotli response headers required by this Unity export. Source build files remain untouched.
The fixed 1920×1080 HTML settings are preserved in deployment.

For a local deployment preview, run `node scripts/pages-build.mjs site` and serve `_site/` over HTTP.
Move an existing `_site` folder aside before running that command again. `_site` is not committed.

The current game has a large initial download; reducing textures/audio and unused assets in Unity
will improve loading times, especially on mobile. This setup fixes hosting, not asset size.
