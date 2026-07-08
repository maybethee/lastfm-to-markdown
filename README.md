# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/mary+in+the+junkyard/Role+Model+Hermit"><img src="https://lastfm.freetls.fastly.net/i/u/174s/8a6d389be4b409fe755e0c4a6427498c.png" title="mary in the junkyard - Role Model Hermit"></a> <a href="https://www.last.fm/music/LEENALCHI/Here+Comes+That+Crow"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3e4544374155a827574891bd66e56670.jpg" title="LEENALCHI - Here Comes That Crow"></a> <a href="https://www.last.fm/music/svn4vr/herts+on+fire"><img src="https://lastfm.freetls.fastly.net/i/u/174s/c123ab3f2748a7b68b0cf8ff48ed0792.png" title="svn4vr - herts on fire"></a> <a href="https://www.last.fm/music/mary+in+the+junkyard/THis+Old+House"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0707c424ee98a99712c563edc397e117.jpg" title="mary in the junkyard - THis Old House"></a> <a href="https://www.last.fm/music/James+Blake/Friends+That+Break+Your+Heart"><img src="https://lastfm.freetls.fastly.net/i/u/174s/97af2e81f45de7e76f7187714f26864d.jpg" title="James Blake - Friends That Break Your Heart"></a> <a href="https://www.last.fm/music/Low/HEY+WHAT"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0b305c6f897c39de5c45fc08b5064679.jpg" title="Low - HEY WHAT"></a> <a href="https://www.last.fm/music/Jessie+Ware/Superbloom"><img src="https://lastfm.freetls.fastly.net/i/u/174s/8ccb5ed69ae70c56f34db09e36590a7f.png" title="Jessie Ware - Superbloom"></a> <a href="https://www.last.fm/music/Pollyfromthedirt/The+dirt+pt.+2"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ebb146d4c696b976083ee20a9162cbff.jpg" title="Pollyfromthedirt - The dirt pt. 2"></a> <a href="https://www.last.fm/music/Rico+Nasty/Cupcake"><img src="https://lastfm.freetls.fastly.net/i/u/174s/2c03bb2376b735556e04bbbd85a0033e.jpg" title="Rico Nasty - Cupcake"></a> <a href="https://www.last.fm/music/Carly+Rae+Jepsen/On+Wires"><img src="https://lastfm.freetls.fastly.net/i/u/174s/24fa2495ef8aea5c8f15ccbddb764457.jpg" title="Carly Rae Jepsen - On Wires"></a> </p>

          
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
