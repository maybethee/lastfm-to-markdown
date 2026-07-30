# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Nia+Archives/Emotional+Junglist"><img src="https://lastfm.freetls.fastly.net/i/u/174s/f7312be542d0bca7310b85de5ca85ebe.jpg" title="Nia Archives - Emotional Junglist"></a> <a href="https://www.last.fm/music/Four+Tet/There+Is+Love+in+You+(Expanded+Edition)"><img src="https://lastfm.freetls.fastly.net/i/u/174s/35df822c9b46df5d156302f1b3ab6852.jpg" title="Four Tet - There Is Love in You (Expanded Edition)"></a> <a href="https://www.last.fm/music/Angel%27in+Heavy+Syrup/IV"><img src="https://lastfm.freetls.fastly.net/i/u/174s/9c1c63c61e09c171d5c875d3e1d12ed9.jpg" title="Angel'in Heavy Syrup - IV"></a> <a href="https://www.last.fm/music/Manos+Hadjidakis/O+Megalos+Erotikos+Horion+O+Pothos"><img src="https://lastfm.freetls.fastly.net/i/u/174s/cacdcf4988513fb0426d4c7348748b9f.jpg" title="Manos Hadjidakis - O Megalos Erotikos Horion O Pothos"></a> <a href="https://www.last.fm/music/Patrik+Fitzgerald/The+Very+Best+of+Patrik+Fitzgerald"><img src="https://lastfm.freetls.fastly.net/i/u/174s/125eabd6a50144fa84fdb5cff6fc701d.jpg" title="Patrik Fitzgerald - The Very Best of Patrik Fitzgerald"></a> <a href="https://www.last.fm/music/1-800+GIRLS/LOVE"><img src="https://lastfm.freetls.fastly.net/i/u/174s/984525b0b7e32453dbc241115b683930.jpg" title="1-800 GIRLS - LOVE"></a> <a href="https://www.last.fm/music/2hollis/hurt"><img src="https://lastfm.freetls.fastly.net/i/u/174s/abe2f798a3fb25d82aa3bf2b98f631d0.jpg" title="2hollis - hurt"></a> <a href="https://www.last.fm/music/Aesop+Rock/Skelethon"><img src="https://lastfm.freetls.fastly.net/i/u/174s/410dd760a834489f8e48ab5fa2968727.png" title="Aesop Rock - Skelethon"></a> <a href="https://www.last.fm/music/Anthony+Green/Pets"><img src="https://lastfm.freetls.fastly.net/i/u/174s/2fce7604d85a52a80408f26ea72f4c70.jpg" title="Anthony Green - Pets"></a> <a href="https://www.last.fm/music/Bibio/Ribbons"><img src="https://lastfm.freetls.fastly.net/i/u/174s/51594c54f89065ba9ddeb2eeff6880ec.jpg" title="Bibio - Ribbons"></a> </p>

          
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
