# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/The+Lemon+Twigs/Look+For+Your+Mind!"><img src="https://lastfm.freetls.fastly.net/i/u/174s/eede61e704e8c18f70103c6e84e359f3.jpg" title="The Lemon Twigs - Look For Your Mind!"></a> <a href="https://www.last.fm/music/King+Krule/6+Feet+Beneath+the+Moon"><img src="https://lastfm.freetls.fastly.net/i/u/174s/32618fce0370ea48771b8e9d4cd47f4f.jpg" title="King Krule - 6 Feet Beneath the Moon"></a> <a href="https://www.last.fm/music/Clazziquai/Instant+Pig"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3987d2787bd280131c0d1e3703c3f50b.jpg" title="Clazziquai - Instant Pig"></a> <a href="https://www.last.fm/music/underscores/U"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82573426631c6de14959f4753eafe666.jpg" title="underscores - U"></a> <a href="https://www.last.fm/music/Arlo+Parks/Ambiguous+Desire"><img src="https://lastfm.freetls.fastly.net/i/u/174s/fc245acc437f29f421abfc3ed1488342.jpg" title="Arlo Parks - Ambiguous Desire"></a> <a href="https://www.last.fm/music/Arlo+Parks/Collapsed+in+Sunbeams"><img src="https://lastfm.freetls.fastly.net/i/u/174s/64502ded79e928c57b7b49c36492144f.jpg" title="Arlo Parks - Collapsed in Sunbeams"></a> <a href="https://www.last.fm/music/Bad+Rabbits/Stick+Up+Kids"><img src="https://lastfm.freetls.fastly.net/i/u/174s/96a617a761834b4cb4e1fda1aa6caf92.jpg" title="Bad Rabbits - Stick Up Kids"></a> <a href="https://www.last.fm/music/Colin+Self/respite+%E2%88%9E+levity+for+the+nameless+ghost+in+crisis"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a4fc92c2d6398780f876c22a0d3fc566.jpg" title="Colin Self - respite ∞ levity for the nameless ghost in crisis"></a> <a href="https://www.last.fm/music/Djrum/Portrait+with+Firewood"><img src="https://lastfm.freetls.fastly.net/i/u/174s/f371a447d98084c8f2a45879f4a6a41b.jpg" title="Djrum - Portrait with Firewood"></a> <a href="https://www.last.fm/music/Jane+Remover/%E2%99%A1"><img src="https://lastfm.freetls.fastly.net/i/u/174s/6fb5b5b2eda71c36cb73d17571251190.jpg" title="Jane Remover - ♡"></a> </p>

          
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
