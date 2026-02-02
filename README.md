# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Tyler+Ballgame/For+The+First+Time,+Again"><img src="https://lastfm.freetls.fastly.net/i/u/174s/876fcf05395f117a1866960499d32720.jpg" title="Tyler Ballgame - For The First Time, Again"></a> <a href="https://www.last.fm/music/Masakatsu+Takagi/Marginalia+III"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82e426334b6dc70b334859933b6ff392.jpg" title="Masakatsu Takagi - Marginalia III"></a> <a href="https://www.last.fm/music/Takashi+Yoshimatsu/Yoshimatsu:+Piano+Concerto+%22Memo+Flora%22"><img src="https://lastfm.freetls.fastly.net/i/u/174s/7a604f8050187bfe375300ce41db64e8.png" title="Takashi Yoshimatsu - Yoshimatsu: Piano Concerto "Memo Flora""></a> <a href="https://www.last.fm/music/Takashi+Yoshimatsu/Yoshimatsu:+Symphony+No.+4,+Trombone+Concerto+&+Atom+Hearts+Club+Suite"><img src="https://lastfm.freetls.fastly.net/i/u/174s/e88637e807e5c0150a1190b4258b91fb.png" title="Takashi Yoshimatsu - Yoshimatsu: Symphony No. 4, Trombone Concerto & Atom Hearts Club Suite"></a> <a href="https://www.last.fm/music/Everything+Everything/Mountainhead"><img src="https://lastfm.freetls.fastly.net/i/u/174s/10e816c8557155a150cb0f20aea2d09d.png" title="Everything Everything - Mountainhead"></a> <a href="https://www.last.fm/music/P.M.+Dawn/Of+The+Heart,+Of+The+Soul+And+Of+The+Cross:+The+Utopian+Experience"><img src="https://lastfm.freetls.fastly.net/i/u/174s/f9ae854ab6a8915eedd6280fe73d6d47.jpg" title="P.M. Dawn - Of The Heart, Of The Soul And Of The Cross: The Utopian Experience"></a> <a href="https://www.last.fm/music/Alabaster+Deplume/To+Cy+&+Lee:+Instrumentals,+Vol.+1"><img src="https://lastfm.freetls.fastly.net/i/u/174s/666a32d3f2259b620c50e25513119b4b.jpg" title="Alabaster Deplume - To Cy & Lee: Instrumentals, Vol. 1"></a> <a href="https://www.last.fm/music/+noredirect/Pan-American/Quiet+City"><img src="https://lastfm.freetls.fastly.net/i/u/174s/b42bb4e135f145b3acb17d662de3c0a1.jpg" title="Pan-American - Quiet City"></a> <a href="https://www.last.fm/music/Sophie/Oil+of+Every+Pearl%27s+Un-Insides"><img src="https://lastfm.freetls.fastly.net/i/u/174s/b482e95ee228abbaaccd0d5a31b81ad2.jpg" title="Sophie - Oil of Every Pearl's Un-Insides"></a> <a href="https://www.last.fm/music/Low/HEY+WHAT"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0b305c6f897c39de5c45fc08b5064679.jpg" title="Low - HEY WHAT"></a> </p>

          
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
