# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Tele+Novella/Poet%27s+Tooth"><img src="https://lastfm-img.freetls.fastly.net/i/u/174s/1d79781b26660d15a1a4e9a1a5b8aed6.jpg" title="Tele Novella - Poet's Tooth"></a> <a href="https://www.last.fm/music/Tele+Novella/Ring+of+Stones"><img src="https://lastfm-img.freetls.fastly.net/i/u/174s/3176316a5d86ce18160b815b8c8bf5f2.jpg" title="Tele Novella - Ring of Stones"></a> <a href="https://www.last.fm/music/Eve+Adams/American+Dust"><img src="https://lastfm-img.freetls.fastly.net/i/u/174s/b00b66c1e7a85dba43841a6c1d969dfd.jpg" title="Eve Adams - American Dust"></a> <a href="https://www.last.fm/music/Eve+Adams/Metal+Bird"><img src="https://lastfm-img.freetls.fastly.net/i/u/174s/1801f1309a7e845382782ae0a0d5dec6.jpg" title="Eve Adams - Metal Bird"></a> <a href="https://www.last.fm/music/8485/Buzzbands"><img src="https://lastfm-img.freetls.fastly.net/i/u/174s/6909cf100be5951b221ed6ede920a10e.jpg" title="8485 - Buzzbands"></a> <a href="https://www.last.fm/music/Ado/%E3%83%A2%E3%83%B3%E3%82%B9%E3%83%88%E3%83%AD"><img src="https://lastfm-img.freetls.fastly.net/i/u/174s/87fdf1001c1a47ea2d146967fe88bb55.jpg" title="Ado - モンストロ"></a> <a href="https://www.last.fm/music/Arlo+Parks/Ambiguous+Desire"><img src="https://lastfm-img.freetls.fastly.net/i/u/174s/fc245acc437f29f421abfc3ed1488342.jpg" title="Arlo Parks - Ambiguous Desire"></a> <a href="https://www.last.fm/music/Carly+Rae+Jepsen/Day+and+Night+%5BExplicit%5D"><img src="https://lastfm-img.freetls.fastly.net/i/u/174s/26a82d77760c865ce35c4431a1d33c81.jpg" title="Carly Rae Jepsen - Day and Night [Explicit]"></a> <a href="https://www.last.fm/music/Fat+Dog/Shit+Love"><img src="https://lastfm-img.freetls.fastly.net/i/u/174s/10f9b6739c7a1ceb963311cc3d4336fd.jpg" title="Fat Dog - Shit Love"></a> <a href="https://www.last.fm/music/Ichiko+Aoba/Shiranui"><img src="https://lastfm-img.freetls.fastly.net/i/u/174s/6ca1020cab1480369b3bd69c415fefc7.jpg" title="Ichiko Aoba - Shiranui"></a> </p>

          
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
