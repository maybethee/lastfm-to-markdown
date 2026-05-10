# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/The+Luyas/Animator"><img src="https://lastfm.freetls.fastly.net/i/u/174s/cb7fb35056ef489cbd7ca63743f83f5f.jpg" title="The Luyas - Animator"></a> <a href="https://www.last.fm/music/Rochelle+Jordan/Through+The+Wall"><img src="https://lastfm.freetls.fastly.net/i/u/174s/abef18b86148b4b4d8961deda9b4942e.png" title="Rochelle Jordan - Through The Wall"></a> <a href="https://www.last.fm/music/Maria+BC/Marathon"><img src="https://lastfm.freetls.fastly.net/i/u/174s/559f7c3d78070286108105db2016086c.jpg" title="Maria BC - Marathon"></a> <a href="https://www.last.fm/music/Ravyn+Lenae/Hypnos"><img src="https://lastfm.freetls.fastly.net/i/u/174s/b92b71c08c39a0aa90f0b43df510dc0b.jpg" title="Ravyn Lenae - Hypnos"></a> <a href="https://www.last.fm/music/Jump+Source/Fold"><img src="https://lastfm.freetls.fastly.net/i/u/174s/39ecdf16f17398ec14a280ce6247df03.jpg" title="Jump Source - Fold"></a> <a href="https://www.last.fm/music/The+Lemon+Twigs/Look+For+Your+Mind!"><img src="https://lastfm.freetls.fastly.net/i/u/174s/eede61e704e8c18f70103c6e84e359f3.jpg" title="The Lemon Twigs - Look For Your Mind!"></a> <a href="https://www.last.fm/music/%E7%8E%8B%E5%BF%86%E7%81%B5/%E6%9E%AF%E8%90%8E%E9%A0%8C"><img src="https://lastfm.freetls.fastly.net/i/u/174s/41d279c33b6e51ce011b9abd5048b635.jpg" title="王忆灵 - 枯萎頌"></a> <a href="https://www.last.fm/music/Joel+Lyssarides/Late+on+Earth"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ee718be3ea4703f203ad7db4f5ba2593.jpg" title="Joel Lyssarides - Late on Earth"></a> <a href="https://www.last.fm/music/Sasha+Keable/ACT+II"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3c20914f2df801dcb8d79a7d1a88c7de.jpg" title="Sasha Keable - ACT II"></a> <a href="https://www.last.fm/music/The+Luyas/Too+Beautiful+to+Work"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3fddfe444e8a40fb9b6064e866b1736c.png" title="The Luyas - Too Beautiful to Work"></a> </p>

          
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
