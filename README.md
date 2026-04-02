# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/No+Party+For+Cao+Dong/%E7%93%A6%E5%90%88"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a2e13cf825769421e4d299c9b6c6ebfd.jpg" title="No Party For Cao Dong - 瓦合"></a> <a href="https://www.last.fm/music/Jane+Remover/%E2%99%A1"><img src="https://lastfm.freetls.fastly.net/i/u/174s/6fb5b5b2eda71c36cb73d17571251190.jpg" title="Jane Remover - ♡"></a> <a href="https://www.last.fm/music/underscores/U"><img src="https://lastfm.freetls.fastly.net/i/u/174s/82573426631c6de14959f4753eafe666.jpg" title="underscores - U"></a> <a href="https://www.last.fm/music/Erika+de+Casier/Essentials"><img src="https://lastfm.freetls.fastly.net/i/u/174s/8913a544c48d5235d434d9b26c480c43.jpg" title="Erika de Casier - Essentials"></a> <a href="https://www.last.fm/music/No+Party+For+Cao+Dong/%E9%86%9C%E5%A5%B4%E5%85%92"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a16553471b8e5083b5d18a88735cbe15.jpg" title="No Party For Cao Dong - 醜奴兒"></a> <a href="https://www.last.fm/music/Courtney+Barnett/Creature+of+Habit"><img src="https://lastfm.freetls.fastly.net/i/u/174s/da18972e0716a9aa5e7533cb44ad988f.jpg" title="Courtney Barnett - Creature of Habit"></a> <a href="https://www.last.fm/music/Erika+de+Casier/Sensational"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0e8e2798a498c100fc3254f507cb28e9.png" title="Erika de Casier - Sensational"></a> <a href="https://www.last.fm/music/Bill+Withers/%27Justments"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0380dc3efd335ed2ecaeb08add29dcfb.jpg" title="Bill Withers - 'Justments"></a> <a href="https://www.last.fm/music/Brian+Finnegan/The+Ravishing+Genius+of+Bones"><img src="https://lastfm.freetls.fastly.net/i/u/174s/4fa6223f1a684610bed0140142d5bfee.jpg" title="Brian Finnegan - The Ravishing Genius of Bones"></a> <a href="https://www.last.fm/music/Robyn/Sexistential"><img src="https://lastfm.freetls.fastly.net/i/u/174s/6dfafe6ee394f5d2bfc5bbab710c15f3.jpg" title="Robyn - Sexistential"></a> </p>

          
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
