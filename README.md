# HENG Facebook Video Publisher

A Netlify-hosted web tool for publishing a normal Facebook Page Feed video post (not Reels).

## Features
- Mobile-friendly video upload and preview
- Caption editor and pre-publish preview
- Resumable upload flow (4 MB browser chunks) through Netlify Functions
- Page token stays server-side; it is never sent to browser JavaScript
- Publish result with Facebook video ID

## Requirements
- A Meta developer app
- A Facebook Page you manage, with the `CREATE_CONTENT` task
- A Page Access Token with `pages_manage_posts`, `pages_read_engagement`, and `pages_show_list`
- Netlify site with Functions enabled

Meta documents publishing a Page video at `POST /{page-id}/videos`. See:
https://developers.facebook.com/docs/graph-api/reference/page/videos

## Configure Netlify
In Netlify, open **Project configuration → Environment variables** and add:
- `META_PAGE_ID`: your Facebook Page ID
- `META_PAGE_ACCESS_TOKEN`: Page Access Token (secret)
- `META_GRAPH_VERSION`: `v27.0` (or a currently supported version for your Meta app)

Do not put the Page Access Token in `public/`, frontend JavaScript, Git, or a public repository.
After changing environment variables, redeploy the site.

## Run locally
1. Install Node.js (LTS).
2. Run `npm install`.
3. Copy `.env.example` to `.env` and fill in the values.
4. Run `npm run dev`.

## Deploy
1. Upload this project to a private GitHub repository or deploy the folder with Netlify.
2. Set the environment variables above in Netlify.
3. Deploy. Open the Netlify URL on iPhone or PC.

## How publishing works
1. `video-start` asks Meta for a resumable upload session.
2. Browser sends video chunks (4 MB each) to `video-transfer`.
3. `video-finish` tells Meta to publish the uploaded video with the caption.
4. The app displays the returned video ID.

The app deliberately does not implement login/token collection. Obtain and manage tokens through Meta's official developer tools. Tokens can expire or be revoked; regenerate them when needed. Meta app review / business verification requirements may apply depending on who uses the app and which permissions are requested.

## Notes
- This starter is configured for one Page ID/token pair per Netlify deployment.
- Only publish content you own or are authorized to post.
- Netlify Functions and Meta API limits may vary. Test first with a short, small video.
- If the API reports a permission or task error, confirm the token belongs to the selected Page and the person has `CREATE_CONTENT`.
