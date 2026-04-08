# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Circa+Survive/On+Letting+Go:+Deluxe+Ten+Year+Edition"><img src="https://lastfm.freetls.fastly.net/i/u/174s/4f5204e83f878a290cc4b9119fe6531a.jpg" title="Circa Survive - On Letting Go: Deluxe Ten Year Edition"></a> <a href="https://www.last.fm/music/Thundercat/Distracted"><img src="https://lastfm.freetls.fastly.net/i/u/174s/5414c61c5f2e8e1dfea513ecd5d70ad0.jpg" title="Thundercat - Distracted"></a> <a href="https://www.last.fm/music/Arlo+Parks/Ambiguous+Desire"><img src="https://lastfm.freetls.fastly.net/i/u/174s/fc245acc437f29f421abfc3ed1488342.jpg" title="Arlo Parks - Ambiguous Desire"></a> <a href="https://www.last.fm/music/L.S.+Dunes/Violet"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3015f0072898fc82fad6555235fe265a.jpg" title="L.S. Dunes - Violet"></a> <a href="https://www.last.fm/music/No+Party+For+Cao+Dong/%E7%93%A6%E5%90%88"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a2e13cf825769421e4d299c9b6c6ebfd.jpg" title="No Party For Cao Dong - 瓦合"></a> <a href="https://www.last.fm/music/Sleeping+With+Sirens/Let%27s+Cheers+to+This"><img src="https://lastfm.freetls.fastly.net/i/u/174s/b7f8129ef56dda79c9472248ae7d561a.jpg" title="Sleeping With Sirens - Let's Cheers to This"></a> <a href="https://www.last.fm/music/James+Blake/Friends+That+Break+Your+Heart"><img src="https://lastfm.freetls.fastly.net/i/u/174s/97af2e81f45de7e76f7187714f26864d.jpg" title="James Blake - Friends That Break Your Heart"></a> <a href="https://www.last.fm/music/Circa+Survive/Juturna"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0b19238d3e46bac4fe3282d586cb91ba.jpg" title="Circa Survive - Juturna"></a> <a href="https://www.last.fm/music/Erika+de+Casier/Still"><img src="https://lastfm.freetls.fastly.net/i/u/174s/1ea382d4ccb3c2499e73c9031c2e7e1b.jpg" title="Erika de Casier - Still"></a> <a href="https://www.last.fm/music/Erika+de+Casier/Sensational"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0e8e2798a498c100fc3254f507cb28e9.png" title="Erika de Casier - Sensational"></a> </p>

          
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
