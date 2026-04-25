# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Honey+Dijon/The+Nightlife"><img src="https://lastfm.freetls.fastly.net/i/u/174s/f7bcb9c4fc8de9aab3773e3e67e8c188.jpg" title="Honey Dijon - The Nightlife"></a> <a href="https://www.last.fm/music/Jessie+Ware/Superbloom"><img src="https://lastfm.freetls.fastly.net/i/u/174s/8ccb5ed69ae70c56f34db09e36590a7f.png" title="Jessie Ware - Superbloom"></a> <a href="https://www.last.fm/music/Nacho+Picasso/S%C3%A9ance+Musique"><img src="https://lastfm.freetls.fastly.net/i/u/174s/cfdc0413e86ba6ca8b647890881fee67.jpg" title="Nacho Picasso - Séance Musique"></a> <a href="https://www.last.fm/music/Arlo+Parks/Ambiguous+Desire"><img src="https://lastfm.freetls.fastly.net/i/u/174s/fc245acc437f29f421abfc3ed1488342.jpg" title="Arlo Parks - Ambiguous Desire"></a> <a href="https://www.last.fm/music/Vega+Trails/Tremors+in+the+Static"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3b2d7541d73791b2a2feed7541d11c74.jpg" title="Vega Trails - Tremors in the Static"></a> <a href="https://www.last.fm/music/Thundercat/Distracted"><img src="https://lastfm.freetls.fastly.net/i/u/174s/5414c61c5f2e8e1dfea513ecd5d70ad0.jpg" title="Thundercat - Distracted"></a> <a href="https://www.last.fm/music/Matt+Corby/Tragic+Magic"><img src="https://lastfm.freetls.fastly.net/i/u/174s/b018b1bf3ae6c6be445bd49dfb7f92cc.jpg" title="Matt Corby - Tragic Magic"></a> <a href="https://www.last.fm/music/Vega+Trails/Sierra+Tracks"><img src="https://lastfm.freetls.fastly.net/i/u/174s/d8cad7e125ada25cc7e008118cb1d0bb.jpg" title="Vega Trails - Sierra Tracks"></a> <a href="https://www.last.fm/music/Jean+Grae/Everything%27s+Fine"><img src="https://lastfm.freetls.fastly.net/i/u/174s/db5a9a602f85dfa97b7c369a096ed951.png" title="Jean Grae - Everything's Fine"></a> <a href="https://www.last.fm/music/Juni+Habel/Evergreen+In+Your+Mind"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82cf9571b945dd55a621816e92bebf11.jpg" title="Juni Habel - Evergreen In Your Mind"></a> </p>

          
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
