# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Arima+Ederra/A+Rush+To+Nowhere"><img src="https://lastfm.freetls.fastly.net/i/u/174s/9122703d03e444c8658bdb200126513c.jpg" title="Arima Ederra - A Rush To Nowhere"></a> <a href="https://www.last.fm/music/Gorillaz/The+Mountain"><img src="https://lastfm.freetls.fastly.net/i/u/174s/2d163e141a0c8a5bafb515a3fd5a6af5.jpg" title="Gorillaz - The Mountain"></a> <a href="https://www.last.fm/music/War+Child+Records/HELP(2)"><img src="https://lastfm.freetls.fastly.net/i/u/174s/d5df09e663e4659046b3ba17028efdce.png" title="War Child Records - HELP(2)"></a> <a href="https://www.last.fm/music/Harry+Styles/Kiss+All+The+Time.+Disco,+Occasionally."><img src="https://lastfm.freetls.fastly.net/i/u/174s/c2bd6cb310d31e39faaa0f206cee4068.png" title="Harry Styles - Kiss All The Time. Disco, Occasionally."></a> <a href="https://www.last.fm/music/Mitski/Nothing%27s+About+to+Happen+to+Me"><img src="https://lastfm.freetls.fastly.net/i/u/174s/5be1b77c3beb502c69a287b36e261996.jpg" title="Mitski - Nothing's About to Happen to Me"></a> <a href="https://www.last.fm/music/Everything+Everything/Mountainhead"><img src="https://lastfm.freetls.fastly.net/i/u/174s/10e816c8557155a150cb0f20aea2d09d.png" title="Everything Everything - Mountainhead"></a> <a href="https://www.last.fm/music/Justin+Bieber/Journals"><img src="https://lastfm.freetls.fastly.net/i/u/174s/4874b5036240e02de2cab94943ee5bd3.jpg" title="Justin Bieber - Journals"></a> <a href="https://www.last.fm/music/Dirty+Projectors/Earth+Crisis"><img src="https://lastfm.freetls.fastly.net/i/u/174s/1825debb24cc9b574dc19fe09dcaa767.jpg" title="Dirty Projectors - Earth Crisis"></a> <a href="https://www.last.fm/music/Mariah+Carey/The+Rarities"><img src="https://lastfm.freetls.fastly.net/i/u/174s/e8271d3e0ad4ecf6e3d30c000c42a736.png" title="Mariah Carey - The Rarities"></a> <a href="https://www.last.fm/music/Becca+Stevens/Wonderbloom"><img src="https://lastfm.freetls.fastly.net/i/u/174s/723141a1da7a115a6f339dbae096bc3d.jpg" title="Becca Stevens - Wonderbloom"></a> </p>

          
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
