# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Puredigitalsilence/Circumfluence"><img src="https://lastfm.freetls.fastly.net/i/u/174s/97a2f43cf8c7488e8be79e9f4035b065.jpg" title="Puredigitalsilence - Circumfluence"></a> <a href="https://www.last.fm/music/2hollis/hurt"><img src="https://lastfm.freetls.fastly.net/i/u/174s/abe2f798a3fb25d82aa3bf2b98f631d0.jpg" title="2hollis - hurt"></a> <a href="https://www.last.fm/music/Anthony+Green/Pets"><img src="https://lastfm.freetls.fastly.net/i/u/174s/2fce7604d85a52a80408f26ea72f4c70.jpg" title="Anthony Green - Pets"></a> <a href="https://www.last.fm/music/Channel+Tres/Walked+In+The+Room"><img src="https://lastfm.freetls.fastly.net/i/u/174s/f87b3c572cfb7da02898069ad5167fe0.jpg" title="Channel Tres - Walked In The Room"></a> <a href="https://www.last.fm/music/Danny+Scott+Lane/Wave+to+Mikey"><img src="https://lastfm.freetls.fastly.net/i/u/174s/003573127ca931982c812d7c183d0bff.jpg" title="Danny Scott Lane - Wave to Mikey"></a> <a href="https://www.last.fm/music/Herbie+Hancock/Head+Hunters"><img src="https://lastfm.freetls.fastly.net/i/u/174s/62a99e8d7872513fba03f4585e88f271.png" title="Herbie Hancock - Head Hunters"></a> <a href="https://www.last.fm/music/Ini+Kamoze/Sly+and+Robbie+Presents+Ini+Kamoze"><img src="https://lastfm.freetls.fastly.net/i/u/174s/dbb862e370e0433b0d0f15b26bbe65d8.jpg" title="Ini Kamoze - Sly and Robbie Presents Ini Kamoze"></a> <a href="https://www.last.fm/music/Retromigration/Ausfahrt+9+EP"><img src="https://lastfm.freetls.fastly.net/i/u/174s/cf1d8bbd0e62b6b2b1f5467410f35201.jpg" title="Retromigration - Ausfahrt 9 EP"></a> <a href="https://www.last.fm/music/Shed/Oderbruch"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ff35486ceaf85a5f01371671984e99ac.jpg" title="Shed - Oderbruch"></a> </p>

          
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
