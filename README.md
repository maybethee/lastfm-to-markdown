# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/James+Blake/Trying+Times"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3f51572c9b758a7fdfef95e40f8691cd.png" title="James Blake - Trying Times"></a> <a href="https://www.last.fm/music/Slayr/Half+Blood+(Bloodluxe)"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3843dd0874586904ef643332806e585d.jpg" title="Slayr - Half Blood (Bloodluxe)"></a> <a href="https://www.last.fm/music/Cibo+Matto/Stereotype+A"><img src="https://lastfm.freetls.fastly.net/i/u/174s/1f8d90a1650c4471c40da27cc4add578.png" title="Cibo Matto - Stereotype A"></a> <a href="https://www.last.fm/music/Erika+de+Casier/Sensational"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0e8e2798a498c100fc3254f507cb28e9.png" title="Erika de Casier - Sensational"></a> <a href="https://www.last.fm/music/Shabaka/Of+The+Earth"><img src="https://lastfm.freetls.fastly.net/i/u/174s/dd924433d8abbd52aa5c9d6f8dab620a.jpg" title="Shabaka - Of The Earth"></a> <a href="https://www.last.fm/music/Dan+Romer/Far+Cry+5+Presents:+Into+the+Flames+(Original+Game+Soundtrack)"><img src="https://lastfm.freetls.fastly.net/i/u/174s/8bc566258d41f77277147a01c3bf90ad.jpg" title="Dan Romer - Far Cry 5 Presents: Into the Flames (Original Game Soundtrack)"></a> <a href="https://www.last.fm/music/Lianne+La+Havas/Disarray"><img src="https://lastfm.freetls.fastly.net/i/u/174s/edf9f47c0cd39fa09a562d73c4631958.jpg" title="Lianne La Havas - Disarray"></a> <a href="https://www.last.fm/music/Everything+But+the+Girl/Eden"><img src="https://lastfm.freetls.fastly.net/i/u/174s/e98c28f4799f4b8bbff55d16ddc6b8bd.jpg" title="Everything But the Girl - Eden"></a> <a href="https://www.last.fm/music/Mon+Laferte/FEMME+FATALE"><img src="https://lastfm.freetls.fastly.net/i/u/174s/91dadc201866327401302cba49911869.jpg" title="Mon Laferte - FEMME FATALE"></a> <a href="https://www.last.fm/music/Tom+Odell/Best+Day+of+My+Life"><img src="https://lastfm.freetls.fastly.net/i/u/174s/46265023d6efaaea94011a2b1a58fc42.jpg" title="Tom Odell - Best Day of My Life"></a> </p>

          
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
