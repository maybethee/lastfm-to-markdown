# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Tyler+Ballgame/For+The+First+Time,+Again"><img src="https://lastfm.freetls.fastly.net/i/u/174s/876fcf05395f117a1866960499d32720.jpg" title="Tyler Ballgame - For The First Time, Again"></a> <a href="https://www.last.fm/music/Yuko+Hara/Mother"><img src="https://lastfm.freetls.fastly.net/i/u/174s/d7e8d48ad8e2f1e20de512c3808d59d7.jpg" title="Yuko Hara - Mother"></a> <a href="https://www.last.fm/music/Zhan%C3%A9/Saturday+Night"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0a8b997a26b22194231879ce9aace26d.png" title="Zhané - Saturday Night"></a> <a href="https://www.last.fm/music/Michiko/Daydreamer"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a4121571c503e2c96c0ff97b11404fbc.jpg" title="Michiko - Daydreamer"></a> <a href="https://www.last.fm/music/Masakatsu+Takagi/Marginalia+III"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82e426334b6dc70b334859933b6ff392.jpg" title="Masakatsu Takagi - Marginalia III"></a> <a href="https://www.last.fm/music/Sam+Wilkes/Driving"><img src="https://lastfm.freetls.fastly.net/i/u/174s/87c79dd45ed1c4b9c310457fd90c0b7d.jpg" title="Sam Wilkes - Driving"></a> <a href="https://www.last.fm/music/Ryokuoushoku+Shakai/pink+blue"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a5f472fcd0e08ad9f1531fc349260bbc.jpg" title="Ryokuoushoku Shakai - pink blue"></a> <a href="https://www.last.fm/music/h+hunt/playing+piano+for+dad"><img src="https://lastfm.freetls.fastly.net/i/u/174s/c31d4641c984751aa588a5df7c37253d.jpg" title="h hunt - playing piano for dad"></a> <a href="https://www.last.fm/music/Kylie+Minogue/Enjoy+Yourself"><img src="https://lastfm.freetls.fastly.net/i/u/174s/30ab0d174c4ca4c4275707f9f2a66aaf.png" title="Kylie Minogue - Enjoy Yourself"></a> <a href="https://www.last.fm/music/Ryokuoushoku+Shakai/Actor"><img src="https://lastfm.freetls.fastly.net/i/u/174s/be38a33a210ee7cbb9ac4e493aaed6e8.jpg" title="Ryokuoushoku Shakai - Actor"></a> </p>

          
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
