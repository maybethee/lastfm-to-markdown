# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Jukebox+the+Ghost/Everything+Under+the+Sun"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ffc3f8a82f9f4cf5e4df0a78bf40d2da.jpg" title="Jukebox the Ghost - Everything Under the Sun"></a> <a href="https://www.last.fm/music/underscores/U"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82573426631c6de14959f4753eafe666.jpg" title="underscores - U"></a> <a href="https://www.last.fm/music/Maria+BC/Marathon"><img src="https://lastfm.freetls.fastly.net/i/u/174s/559f7c3d78070286108105db2016086c.jpg" title="Maria BC - Marathon"></a> <a href="https://www.last.fm/music/Jukebox+the+Ghost/Let+Live+&+Let+Ghosts"><img src="https://lastfm.freetls.fastly.net/i/u/174s/57b141ab9179e3d30267fc49a8808c56.jpg" title="Jukebox the Ghost - Let Live & Let Ghosts"></a> <a href="https://www.last.fm/music/No+Party+For+Cao+Dong/%E7%93%A6%E5%90%88"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a2e13cf825769421e4d299c9b6c6ebfd.jpg" title="No Party For Cao Dong - 瓦合"></a> <a href="https://www.last.fm/music/Dosh/The+Lost+Take"><img src="https://lastfm.freetls.fastly.net/i/u/174s/af9405e857b5445182d147bdf7fb4320.jpg" title="Dosh - The Lost Take"></a> <a href="https://www.last.fm/music/Joel+Lyssarides/Late+on+Earth"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ee718be3ea4703f203ad7db4f5ba2593.jpg" title="Joel Lyssarides - Late on Earth"></a> <a href="https://www.last.fm/music/No+Party+For+Cao+Dong/%E9%86%9C%E5%A5%B4%E5%85%92"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a16553471b8e5083b5d18a88735cbe15.jpg" title="No Party For Cao Dong - 醜奴兒"></a> <a href="https://www.last.fm/music/Sasha+Keable/ACT+II"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3c20914f2df801dcb8d79a7d1a88c7de.jpg" title="Sasha Keable - ACT II"></a> <a href="https://www.last.fm/music/Ravyn+Lenae/Bird%27s+Eye"><img src="https://lastfm.freetls.fastly.net/i/u/174s/abf6d2550d60aecebc27bf20d77c3462.jpg" title="Ravyn Lenae - Bird's Eye"></a> </p>

          
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
