# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Judeline/BODHIRIA"><img src="https://lastfm.freetls.fastly.net/i/u/174s/7408ed008b4d9ff47a35d2d7c628761a.jpg" title="Judeline - BODHIRIA"></a> <a href="https://www.last.fm/music/Chinese+Football/Chinese+Football"><img src="https://lastfm.freetls.fastly.net/i/u/174s/67725d061c81056c743ff4250445b652.jpg" title="Chinese Football - Chinese Football"></a> <a href="https://www.last.fm/music/McKinley+Dixon/Beloved!+Paradise!+Jazz!%3F"><img src="https://lastfm.freetls.fastly.net/i/u/174s/f4e77dd10e114f7b57b418e5ec47835e.png" title="McKinley Dixon - Beloved! Paradise! Jazz!?"></a> <a href="https://www.last.fm/music/Mount+Eerie/Dawn"><img src="https://lastfm.freetls.fastly.net/i/u/174s/48a13b8689d67f28d21902b79dd10c31.jpg" title="Mount Eerie - Dawn"></a> <a href="https://www.last.fm/music/Miniature+Tigers/Summer+of+the+Cow"><img src="https://lastfm.freetls.fastly.net/i/u/174s/4803c404c78719910c6f31f3aee74d4c.jpg" title="Miniature Tigers - Summer of the Cow"></a> <a href="https://www.last.fm/music/The+Lemon+Twigs/Look+For+Your+Mind!"><img src="https://lastfm.freetls.fastly.net/i/u/174s/eede61e704e8c18f70103c6e84e359f3.jpg" title="The Lemon Twigs - Look For Your Mind!"></a> <a href="https://www.last.fm/music/Jane+Remover/%E2%99%A1"><img src="https://lastfm.freetls.fastly.net/i/u/174s/6fb5b5b2eda71c36cb73d17571251190.jpg" title="Jane Remover - ♡"></a> <a href="https://www.last.fm/music/The+Lemon+Twigs/Everything+Harmony"><img src="https://lastfm.freetls.fastly.net/i/u/174s/9956c0d84e3aa13b6af118359c27abc4.jpg" title="The Lemon Twigs - Everything Harmony"></a> <a href="https://www.last.fm/music/underscores/U"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82573426631c6de14959f4753eafe666.jpg" title="underscores - U"></a> <a href="https://www.last.fm/music/Circa+Survive/On+Letting+Go:+Deluxe+Ten+Year+Edition"><img src="https://lastfm.freetls.fastly.net/i/u/174s/4f5204e83f878a290cc4b9119fe6531a.jpg" title="Circa Survive - On Letting Go: Deluxe Ten Year Edition"></a> </p>

          
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
