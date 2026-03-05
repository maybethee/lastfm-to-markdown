# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Jill+Scott/To+Whom+This+May+Concern"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ef3db5bf387928d95b6e13cc473949f5.jpg" title="Jill Scott - To Whom This May Concern"></a> <a href="https://www.last.fm/music/Gorillaz/The+Mountain"><img src="https://lastfm.freetls.fastly.net/i/u/174s/2d163e141a0c8a5bafb515a3fd5a6af5.jpg" title="Gorillaz - The Mountain"></a> <a href="https://www.last.fm/music/MJ+Lenderman/Manning+Fireworks"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a2c44050058fb5260a113b962dec2bc2.png" title="MJ Lenderman - Manning Fireworks"></a> <a href="https://www.last.fm/music/Friendship/Caveman+Wakes+Up"><img src="https://lastfm.freetls.fastly.net/i/u/174s/1001a4966fd4a23af1cf366aa99514e8.jpg" title="Friendship - Caveman Wakes Up"></a> <a href="https://www.last.fm/music/Dirty+Projectors/Earth+Crisis"><img src="https://lastfm.freetls.fastly.net/i/u/174s/1825debb24cc9b574dc19fe09dcaa767.jpg" title="Dirty Projectors - Earth Crisis"></a> <a href="https://www.last.fm/music/Little+Comets/Life+Is+Elsewhere"><img src="https://lastfm.freetls.fastly.net/i/u/174s/2dc6ed16d8f3452eb80a945adbe546a6.jpg" title="Little Comets - Life Is Elsewhere"></a> <a href="https://www.last.fm/music/Mariah+Carey/The+Rarities"><img src="https://lastfm.freetls.fastly.net/i/u/174s/e8271d3e0ad4ecf6e3d30c000c42a736.png" title="Mariah Carey - The Rarities"></a> <a href="https://www.last.fm/music/Tierra+Whack/Whack+World"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3c277d9b4c5f40156e013a490818dcec.jpg" title="Tierra Whack - Whack World"></a> <a href="https://www.last.fm/music/Mammal+Hands/Circadia"><img src="https://lastfm.freetls.fastly.net/i/u/174s/5eced657c5465f9ab23af5fd8cbf1942.jpg" title="Mammal Hands - Circadia"></a> <a href="https://www.last.fm/music/Masakatsu+Takagi/Marginalia+VII"><img src="https://lastfm.freetls.fastly.net/i/u/174s/b27cf513abf06bb5cddb88755d2b9bab.jpg" title="Masakatsu Takagi - Marginalia VII"></a> </p>

          
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
