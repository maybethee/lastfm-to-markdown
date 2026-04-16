# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Nourished+by+Time/Erotic+Probiotic+2"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3319e858f36821112556beacd7dcbf9a.jpg" title="Nourished by Time - Erotic Probiotic 2"></a> <a href="https://www.last.fm/music/We+Are+The+City/Violent"><img src="https://lastfm.freetls.fastly.net/i/u/174s/fc35a9bdf60d40e382de6b80f7a65596.jpg" title="We Are The City - Violent"></a> <a href="https://www.last.fm/music/Peven+Everett/Power+Soul"><img src="https://lastfm.freetls.fastly.net/i/u/174s/8c4579bc24a0433385ff5c3d0a65db29.jpg" title="Peven Everett - Power Soul"></a> <a href="https://www.last.fm/music/Slayyyter/WOR$T+GIRL+IN+AMERICA"><img src="https://lastfm.freetls.fastly.net/i/u/174s/d60a52a2d3e8eec7d9df29dc18d16ec2.png" title="Slayyyter - WOR$T GIRL IN AMERICA"></a> <a href="https://www.last.fm/music/Spiritual+Cramp/RUDE"><img src="https://lastfm.freetls.fastly.net/i/u/174s/cd4feec08e5f8e002c5421d0f8a4147d.jpg" title="Spiritual Cramp - RUDE"></a> <a href="https://www.last.fm/music/Laufey/A+Matter+of+Time:+The+Final+Hour"><img src="https://lastfm.freetls.fastly.net/i/u/174s/69200da2dfd46b078eefb15821f935ca.png" title="Laufey - A Matter of Time: The Final Hour"></a> <a href="https://www.last.fm/music/We+Are+The+City/High+School"><img src="https://lastfm.freetls.fastly.net/i/u/174s/23b29ef9144e41d2967a6eb373fdbf30.jpg" title="We Are The City - High School"></a> <a href="https://www.last.fm/music/We+Are+The+City/Above+Club"><img src="https://lastfm.freetls.fastly.net/i/u/174s/26858f3486649cec103489dad8476d88.jpg" title="We Are The City - Above Club"></a> <a href="https://www.last.fm/music/Vega+Trails/Sierra+Tracks"><img src="https://lastfm.freetls.fastly.net/i/u/174s/d8cad7e125ada25cc7e008118cb1d0bb.jpg" title="Vega Trails - Sierra Tracks"></a> <a href="https://www.last.fm/music/Circa+Survive/On+Letting+Go:+Deluxe+Ten+Year+Edition"><img src="https://lastfm.freetls.fastly.net/i/u/174s/4f5204e83f878a290cc4b9119fe6531a.jpg" title="Circa Survive - On Letting Go: Deluxe Ten Year Edition"></a> </p>

          
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
