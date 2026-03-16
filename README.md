# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/James+Blake/Trying+Times"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3f51572c9b758a7fdfef95e40f8691cd.png" title="James Blake - Trying Times"></a> <a href="https://www.last.fm/music/Masakatsu+Takagi/Marginalia+III"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82e426334b6dc70b334859933b6ff392.jpg" title="Masakatsu Takagi - Marginalia III"></a> <a href="https://www.last.fm/music/PlayRadioPlay!/Texas"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ea7292358b7c496493765ccd140a9738.jpg" title="PlayRadioPlay! - Texas"></a> <a href="https://www.last.fm/music/Momoko+Gill/Momoko"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0dff43b8fa3ea46554811e696986626a.jpg" title="Momoko Gill - Momoko"></a> <a href="https://www.last.fm/music/Ravyn+Lenae/Bird%27s+Eye"><img src="https://lastfm.freetls.fastly.net/i/u/174s/abf6d2550d60aecebc27bf20d77c3462.jpg" title="Ravyn Lenae - Bird's Eye"></a> <a href="https://www.last.fm/music/Everything+Everything/Man+Alive"><img src="https://lastfm.freetls.fastly.net/i/u/174s/cdcf85afc8c7dd5a13dee7137a9a71ec.png" title="Everything Everything - Man Alive"></a> <a href="https://www.last.fm/music/Low/HEY+WHAT"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0b305c6f897c39de5c45fc08b5064679.jpg" title="Low - HEY WHAT"></a> <a href="https://www.last.fm/music/Djrum/Under+Tangled+Silence"><img src="https://lastfm.freetls.fastly.net/i/u/174s/e419046021c8cbd6e6417ab6884d0a78.jpg" title="Djrum - Under Tangled Silence"></a> <a href="https://www.last.fm/music/Analog+Rebellion/Cavanaugh,+Something+(Pre-Sides+and+Varieties)+-+EP"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3eb13ec8f3d47e309e6586ca10dccc83.jpg" title="Analog Rebellion - Cavanaugh, Something (Pre-Sides and Varieties) - EP"></a> <a href="https://www.last.fm/music/Hania+Rani/Sentimental+Value"><img src="https://lastfm.freetls.fastly.net/i/u/174s/10f74722cb2106c15d382697dceb91d5.jpg" title="Hania Rani - Sentimental Value"></a> </p>

          
## 👩🏽‍💻 What you'll need
* A README.md file.
* Last.fm API key
  * Fill [this form](https://www.last.fm/api/account/create) to instantly get one. Requires a last.fm account.
* Set up a GitHub Secret called ```LASTFM_API_KEY``` with the value given by last.fm.
* Also set up a ```LASTFM_USER``` GitHub Secret with the user you'll get the weekly charts for.
* Add a ```<!-- lastfm -->``` tag in your README.md file, with two blank lines below it. The album covers will be placed here.

## Instructions
To use this release, add a ```lastfm.yml``` workflow file to the ```.github/workflows``` folder in your repository with the following code:
```diff
name: lastfm-to-markdown

on:
  schedule:
    - cron: '2 0 * * *'
  workflow_dispatch:

jobs:
  lastfm:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: lastfm to markdown
        uses: melipass/lastfm-to-markdown@v1.3.1
        with:
          LASTFM_API_KEY: ${{ secrets.LASTFM_API_KEY }}
          LASTFM_USER: ${{ secrets.LASTFM_USER }}
#         INCLUDE_LINK: true # Optional. Defaults is false. If you want to include the link to the album page, set this to true.
#         IMAGE_COUNT: 6 # Optional. Defaults to 10. Feel free to remove this line if you want.
      - name: commit changes
        continue-on-error: true
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add -A
          git commit -m "Updated last.fm's weekly chart" -a

      - name: push changes
        continue-on-error: true
        uses: ad-m/github-push-action@v0.6.0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}\
          branch: main
```
The cron job is scheduled to run once a day because Last.fm's API updates weekly chart data daily at 00:00, it's useless to make more than 1 request per day because you'll get the same information back every time. You can manually run the workflow in case Last.fm's API was down at the time, going to the Actions tab in your repository.

## 🚧 To do
* Allow users to choose the image size for the album covers.
* Feel free to open an issue or send a pull request for anything you believe would be useful.
