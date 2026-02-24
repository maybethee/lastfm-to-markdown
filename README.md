# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Hemlocke+Springs/the+apple+tree+under+the+sea"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0587d69b8b7bffe55adcf922ddb3af95.png" title="Hemlocke Springs - the apple tree under the sea"></a> <a href="https://www.last.fm/music/Ravyn+Lenae/Bird%27s+Eye"><img src="https://lastfm.freetls.fastly.net/i/u/174s/abf6d2550d60aecebc27bf20d77c3462.jpg" title="Ravyn Lenae - Bird's Eye"></a> <a href="https://www.last.fm/music/Willow/petal+rock+black"><img src="https://lastfm.freetls.fastly.net/i/u/174s/d9662f7967016323f2f498939c2acf6e.png" title="Willow - petal rock black"></a> <a href="https://www.last.fm/music/Juni+Habel/All+Ears"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82eccaadcdda9a749728f019b8690f66.jpg" title="Juni Habel - All Ears"></a> <a href="https://www.last.fm/music/Freddie+Gibbs/Pi%C3%B1ata+(Alex+Goose+Remix)"><img src="https://lastfm.freetls.fastly.net/i/u/174s/4de9003655d3d53c2dde2fd0ef93efaf.jpg" title="Freddie Gibbs - Piñata (Alex Goose Remix)"></a> <a href="https://www.last.fm/music/Hilary+Duff/luck...+or+something"><img src="https://lastfm.freetls.fastly.net/i/u/174s/1d8b383f7b55919425d4699494be830f.png" title="Hilary Duff - luck... or something"></a> <a href="https://www.last.fm/music/Steve+Reich/Works+1965-1995"><img src="https://lastfm.freetls.fastly.net/i/u/174s/804d1659bc68406891cd7c74eb8ff4a6.jpg" title="Steve Reich - Works 1965-1995"></a> <a href="https://www.last.fm/music/Dent+May/Across+The+Multiverse"><img src="https://lastfm.freetls.fastly.net/i/u/174s/189a45082b20052256ca960436d5063b.jpg" title="Dent May - Across The Multiverse"></a> <a href="https://www.last.fm/music/Sasha+Keable/ACT+II"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3c20914f2df801dcb8d79a7d1a88c7de.jpg" title="Sasha Keable - ACT II"></a> <a href="https://www.last.fm/music/Brian+Finnegan/The+Ravishing+Genius+of+Bones"><img src="https://lastfm.freetls.fastly.net/i/u/174s/4fa6223f1a684610bed0140142d5bfee.jpg" title="Brian Finnegan - The Ravishing Genius of Bones"></a> </p>

          
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
