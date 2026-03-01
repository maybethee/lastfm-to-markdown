# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Hemlocke+Springs/the+apple+tree+under+the+sea"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0587d69b8b7bffe55adcf922ddb3af95.png" title="Hemlocke Springs - the apple tree under the sea"></a> <a href="https://www.last.fm/music/Jill+Scott/To+Whom+This+May+Concern"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ef3db5bf387928d95b6e13cc473949f5.jpg" title="Jill Scott - To Whom This May Concern"></a> <a href="https://www.last.fm/music/The+Lemon+Twigs/Everything+Harmony"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0d0f9c63d31d831108783ecbd46af76b.jpg" title="The Lemon Twigs - Everything Harmony"></a> <a href="https://www.last.fm/music/Ravyn+Lenae/Bird%27s+Eye"><img src="https://lastfm.freetls.fastly.net/i/u/174s/abf6d2550d60aecebc27bf20d77c3462.jpg" title="Ravyn Lenae - Bird's Eye"></a> <a href="https://www.last.fm/music/Richard+Dawson/2020"><img src="https://lastfm.freetls.fastly.net/i/u/174s/b9ce0cb228f7dd3a4463f88be9506c91.jpg" title="Richard Dawson - 2020"></a> <a href="https://www.last.fm/music/MJ+Lenderman/Manning+Fireworks"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a2c44050058fb5260a113b962dec2bc2.png" title="MJ Lenderman - Manning Fireworks"></a> <a href="https://www.last.fm/music/Friendship/Caveman+Wakes+Up"><img src="https://lastfm.freetls.fastly.net/i/u/174s/1001a4966fd4a23af1cf366aa99514e8.jpg" title="Friendship - Caveman Wakes Up"></a> <a href="https://www.last.fm/music/Little+Comets/Life+Is+Elsewhere"><img src="https://lastfm.freetls.fastly.net/i/u/174s/2dc6ed16d8f3452eb80a945adbe546a6.jpg" title="Little Comets - Life Is Elsewhere"></a> <a href="https://www.last.fm/music/Tierra+Whack/Whack+World"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3c277d9b4c5f40156e013a490818dcec.jpg" title="Tierra Whack - Whack World"></a> <a href="https://www.last.fm/music/Masakatsu+Takagi/Marginalia+VII"><img src="https://lastfm.freetls.fastly.net/i/u/174s/b27cf513abf06bb5cddb88755d2b9bab.jpg" title="Masakatsu Takagi - Marginalia VII"></a> </p>

          
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
