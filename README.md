# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/James+Blake/Trying+Times"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3f51572c9b758a7fdfef95e40f8691cd.png" title="James Blake - Trying Times"></a> <a href="https://www.last.fm/music/Masakatsu+Takagi/Marginalia+III"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82e426334b6dc70b334859933b6ff392.jpg" title="Masakatsu Takagi - Marginalia III"></a> <a href="https://www.last.fm/music/Lianne+La+Havas/Disarray"><img src="https://lastfm.freetls.fastly.net/i/u/174s/edf9f47c0cd39fa09a562d73c4631958.jpg" title="Lianne La Havas - Disarray"></a> <a href="https://www.last.fm/music/Shabaka/Of+The+Earth"><img src="https://lastfm.freetls.fastly.net/i/u/174s/dd924433d8abbd52aa5c9d6f8dab620a.jpg" title="Shabaka - Of The Earth"></a> <a href="https://www.last.fm/music/The+Lemon+Twigs/I+Just+Can%27t+Get+Over+Losing+You"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0cf3734e3f5e42b4365b5289b154fb2f.jpg" title="The Lemon Twigs - I Just Can't Get Over Losing You"></a> <a href="https://www.last.fm/music/Tom+Odell/Best+Day+of+My+Life"><img src="https://lastfm.freetls.fastly.net/i/u/174s/46265023d6efaaea94011a2b1a58fc42.jpg" title="Tom Odell - Best Day of My Life"></a> <a href="https://www.last.fm/music/Bill+Callahan/My+Days+of+58"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0589ce33aab919cac4a73a5a23c9b5b6.jpg" title="Bill Callahan - My Days of 58"></a> <a href="https://www.last.fm/music/Bladee/Crest"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3a0aa3e03cbd5467297f397e67a80cd4.png" title="Bladee - Crest"></a> <a href="https://www.last.fm/music/Darwin+Deez/Double+Down"><img src="https://lastfm.freetls.fastly.net/i/u/174s/29ba05c75c48bf9a058090e8b10f58cd.jpg" title="Darwin Deez - Double Down"></a> <a href="https://www.last.fm/music/Alex+G/We%27re+All+Going+to+the+World%27s+Fair+(Original+Motion+Picture+Soundtrack)"><img src="https://lastfm.freetls.fastly.net/i/u/174s/c429b2a6c02f1465085f3384c5236759.jpg" title="Alex G - We're All Going to the World's Fair (Original Motion Picture Soundtrack)"></a> </p>

          
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
