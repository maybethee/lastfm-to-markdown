# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Dolphin+Hyperspace/Echolocation"><img src="https://lastfm.freetls.fastly.net/i/u/174s/b91221ae9fab05faa7ffc2a467d20597.jpg" title="Dolphin Hyperspace - Echolocation"></a> <a href="https://www.last.fm/music/Genesis+Owusu/REDSTAR+WU+&+THE+WORLDWIDE+SCOURGE"><img src="https://lastfm.freetls.fastly.net/i/u/174s/2046c6f5c75e2704e665f88b9e8f0da2.jpg" title="Genesis Owusu - REDSTAR WU & THE WORLDWIDE SCOURGE"></a> <a href="https://www.last.fm/music/No+Party+For+Cao+Dong/%E7%93%A6%E5%90%88"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a2e13cf825769421e4d299c9b6c6ebfd.jpg" title="No Party For Cao Dong - 瓦合"></a> <a href="https://www.last.fm/music/gupi/ever+since"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0722840c6107bed4560e206a566b41a6.png" title="gupi - ever since"></a> <a href="https://www.last.fm/music/No+Party+For+Cao+Dong/%E9%86%9C%E5%A5%B4%E5%85%92"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a16553471b8e5083b5d18a88735cbe15.jpg" title="No Party For Cao Dong - 醜奴兒"></a> <a href="https://www.last.fm/music/Bialystocks/%E3%83%93%E3%82%A2%E3%83%AA%E3%82%B9%E3%83%88%E3%83%83%E3%82%AF%E3%82%B9"><img src="https://lastfm.freetls.fastly.net/i/u/174s/98eb695268ca44cf594f950a2af9a9ca.jpg" title="Bialystocks - ビアリストックス"></a> <a href="https://www.last.fm/music/Honey+Dijon/The+Nightlife"><img src="https://lastfm.freetls.fastly.net/i/u/174s/f7bcb9c4fc8de9aab3773e3e67e8c188.jpg" title="Honey Dijon - The Nightlife"></a> <a href="https://www.last.fm/music/Taylor+Eigsti/Tree+Falls"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a14177f73d89dc8f8feeb8b6fd472a01.jpg" title="Taylor Eigsti - Tree Falls"></a> <a href="https://www.last.fm/music/Ella+Mai/Do+You+Still+Love+Me%3F"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3b7f0be27996d483ac09c7e003570628.png" title="Ella Mai - Do You Still Love Me?"></a> <a href="https://www.last.fm/music/Future+Islands/From+a+Hole+in+the+Floor+to+a+Fountain+of+Youth"><img src="https://lastfm.freetls.fastly.net/i/u/174s/d6c31fbfa269d74fe8ffb022b26102dd.png" title="Future Islands - From a Hole in the Floor to a Fountain of Youth"></a> </p>

          
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
