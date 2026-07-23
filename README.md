# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/The+Gloaming/The+Gloaming"><img src="https://lastfm.freetls.fastly.net/i/u/174s/9ee83c4b3f8f47e2c73bb3356bbf98f3.jpg" title="The Gloaming - The Gloaming"></a> <a href="https://www.last.fm/music/Four+Tet/There+Is+Love+in+You+(Expanded+Edition)"><img src="https://lastfm.freetls.fastly.net/i/u/174s/35df822c9b46df5d156302f1b3ab6852.jpg" title="Four Tet - There Is Love in You (Expanded Edition)"></a> <a href="https://www.last.fm/music/mary+in+the+junkyard/Role+Model+Hermit"><img src="https://lastfm.freetls.fastly.net/i/u/174s/8a6d389be4b409fe755e0c4a6427498c.png" title="mary in the junkyard - Role Model Hermit"></a> <a href="https://www.last.fm/music/Pollyfromthedirt/The+dirt+pt.+2"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ebb146d4c696b976083ee20a9162cbff.jpg" title="Pollyfromthedirt - The dirt pt. 2"></a> <a href="https://www.last.fm/music/The+Gloaming/2"><img src="https://lastfm.freetls.fastly.net/i/u/174s/627a057ae88846da72f53797c3e83a7e.jpg" title="The Gloaming - 2"></a> <a href="https://www.last.fm/music/Kelela/new+avatar"><img src="https://lastfm.freetls.fastly.net/i/u/174s/87fcdd05c11d6fe8cf01ca6036848a93.png" title="Kelela - new avatar"></a> <a href="https://www.last.fm/music/Yard+Act/You%27re+Gonna+Need+A+Little+Music"><img src="https://lastfm.freetls.fastly.net/i/u/174s/28791f4bb20b33dddc445f4b9a8591a8.jpg" title="Yard Act - You're Gonna Need A Little Music"></a> <a href="https://www.last.fm/music/Mammal+Hands/Captured+Spirits"><img src="https://lastfm.freetls.fastly.net/i/u/174s/9463ee56d43a8e366d4d4a255041ce87.jpg" title="Mammal Hands - Captured Spirits"></a> <a href="https://www.last.fm/music/mary+in+the+junkyard/THis+Old+House"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0707c424ee98a99712c563edc397e117.jpg" title="mary in the junkyard - THis Old House"></a> <a href="https://www.last.fm/music/Mammal+Hands/Floa"><img src="https://lastfm.freetls.fastly.net/i/u/174s/01125fe8293d5a94327ecdf6b2f5f7d0.jpg" title="Mammal Hands - Floa"></a> </p>

          
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
