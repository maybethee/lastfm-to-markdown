# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/CFCF/L.U.V."><img src="https://lastfm.freetls.fastly.net/i/u/174s/17a878637b55cb707ac72380b17230c7.jpg" title="CFCF - L.U.V."></a> <a href="https://www.last.fm/music/Tierra+Whack/WHACK%27S+MUSEUM"><img src="https://lastfm.freetls.fastly.net/i/u/174s/cda2e1531ca6f00513f02f3c61be7af4.png" title="Tierra Whack - WHACK'S MUSEUM"></a> <a href="https://www.last.fm/music/nomi./sweet+talk"><img src="https://lastfm.freetls.fastly.net/i/u/174s/44003d206797b413a613d85fb0cf6f0e.jpg" title="nomi. - sweet talk"></a> <a href="https://www.last.fm/music/Andris+Mattson/the+understory"><img src="https://lastfm.freetls.fastly.net/i/u/174s/fba1ca10af3cf818bc73674641fdeb6d.jpg" title="Andris Mattson - the understory"></a> <a href="https://www.last.fm/music/mary+in+the+junkyard/Mouse"><img src="https://lastfm.freetls.fastly.net/i/u/174s/59b266777da247cb39b78fd71eeb8d30.jpg" title="mary in the junkyard - Mouse"></a> <a href="https://www.last.fm/music/Masakatsu+Takagi/Marginalia+III"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82e426334b6dc70b334859933b6ff392.jpg" title="Masakatsu Takagi - Marginalia III"></a> <a href="https://www.last.fm/music/Andris+Mattson/FLUGEL"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0abd70f679ec6a583f47c938589f9cfa.jpg" title="Andris Mattson - FLUGEL"></a> <a href="https://www.last.fm/music/John+Tejada/Shade"><img src="https://lastfm.freetls.fastly.net/i/u/174s/1c24f9b236effe20a6d9e99cdafeed29.jpg" title="John Tejada - Shade"></a> <a href="https://www.last.fm/music/Dirty+Projectors/Dirty+Projectors"><img src="https://lastfm.freetls.fastly.net/i/u/174s/e533ceb55324ab0a73001471421a5932.png" title="Dirty Projectors - Dirty Projectors"></a> <a href="https://www.last.fm/music/Andrs/get+lit"><img src="https://lastfm.freetls.fastly.net/i/u/174s/f4f2c2507971a83f25b0f1b5586180f6.jpg" title="Andrs - get lit"></a> </p>

          
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
