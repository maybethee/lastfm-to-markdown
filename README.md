# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Dove+Ellis/Blizzard"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0fa05e77dd827dcbba7f221b57358900.jpg" title="Dove Ellis - Blizzard"></a> <a href="https://www.last.fm/music/Daphni/Butterfly"><img src="https://lastfm.freetls.fastly.net/i/u/174s/e57f4b2e1ffb55e26c4d3762bca81bc3.png" title="Daphni - Butterfly"></a> <a href="https://www.last.fm/music/Ella+Mai/Do+You+Still+Love+Me%3F"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3b7f0be27996d483ac09c7e003570628.png" title="Ella Mai - Do You Still Love Me?"></a> <a href="https://www.last.fm/music/Yuko+Hara/Mother"><img src="https://lastfm.freetls.fastly.net/i/u/174s/d7e8d48ad8e2f1e20de512c3808d59d7.jpg" title="Yuko Hara - Mother"></a> <a href="https://www.last.fm/music/Maro/SO+MUCH+HAS+CHANGED"><img src="https://lastfm.freetls.fastly.net/i/u/174s/2b6662400fee922d317d7748ee6d541b.jpg" title="Maro - SO MUCH HAS CHANGED"></a> <a href="https://www.last.fm/music/Zhan%C3%A9/Saturday+Night"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0a8b997a26b22194231879ce9aace26d.png" title="Zhané - Saturday Night"></a> <a href="https://www.last.fm/music/Michiko/Daydreamer"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a4121571c503e2c96c0ff97b11404fbc.jpg" title="Michiko - Daydreamer"></a> <a href="https://www.last.fm/music/April+Magazine/If+The+Ceiling+Were+A+Kite:+Vol.+1"><img src="https://lastfm.freetls.fastly.net/i/u/174s/aefbe51318273108a84eccf7ad6d15c1.jpg" title="April Magazine - If The Ceiling Were A Kite: Vol. 1"></a> <a href="https://www.last.fm/music/James+Yuill/Turning+Down+Water+For+Air"><img src="https://lastfm.freetls.fastly.net/i/u/174s/5356b3501a95495cab257f09d789d604.jpg" title="James Yuill - Turning Down Water For Air"></a> <a href="https://www.last.fm/music/Sasha+Keable/ACT+II"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3c20914f2df801dcb8d79a7d1a88c7de.jpg" title="Sasha Keable - ACT II"></a> </p>

          
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
