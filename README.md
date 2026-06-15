# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Judeline/BODHIRIA"><img src="https://lastfm.freetls.fastly.net/i/u/174s/7408ed008b4d9ff47a35d2d7c628761a.jpg" title="Judeline - BODHIRIA"></a> <a href="https://www.last.fm/music/McKinley+Dixon/Beloved!+Paradise!+Jazz!%3F"><img src="https://lastfm.freetls.fastly.net/i/u/174s/f4e77dd10e114f7b57b418e5ec47835e.png" title="McKinley Dixon - Beloved! Paradise! Jazz!?"></a> <a href="https://www.last.fm/music/Mount+Eerie/Dawn"><img src="https://lastfm.freetls.fastly.net/i/u/174s/48a13b8689d67f28d21902b79dd10c31.jpg" title="Mount Eerie - Dawn"></a> <a href="https://www.last.fm/music/Justin+Bieber/Journals"><img src="https://lastfm.freetls.fastly.net/i/u/174s/4874b5036240e02de2cab94943ee5bd3.jpg" title="Justin Bieber - Journals"></a> <a href="https://www.last.fm/music/Olivia+Rodrigo/you+seem+pretty+sad+for+a+girl+so+in+love"><img src="https://lastfm.freetls.fastly.net/i/u/174s/06ccc3cc0cfe1e4ca42a26ac00d97a7b.jpg" title="Olivia Rodrigo - you seem pretty sad for a girl so in love"></a> <a href="https://www.last.fm/music/Sleeping+With+Sirens/An+Ending+In+Itself"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ca1ed4dfa8512de854eab8a1b08e4571.jpg" title="Sleeping With Sirens - An Ending In Itself"></a> <a href="https://www.last.fm/music/Tierra+Whack/Whack+World"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3c277d9b4c5f40156e013a490818dcec.jpg" title="Tierra Whack - Whack World"></a> <a href="https://www.last.fm/music/Clarence+Clarity/VANISHING+ACT+II:+ULTIMATE+REALITY"><img src="https://lastfm.freetls.fastly.net/i/u/174s/309e705a3cc5e47151e33a438041067c.jpg" title="Clarence Clarity - VANISHING ACT II: ULTIMATE REALITY"></a> <a href="https://www.last.fm/music/Miniature+Tigers/I+Dreamt+I+Was+A+Cowboy"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0b74433cb51d8f561dc23675bb5aca5b.jpg" title="Miniature Tigers - I Dreamt I Was A Cowboy"></a> <a href="https://www.last.fm/music/No+Party+For+Cao+Dong/%E7%93%A6%E5%90%88"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a2e13cf825769421e4d299c9b6c6ebfd.jpg" title="No Party For Cao Dong - 瓦合"></a> </p>

          
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
