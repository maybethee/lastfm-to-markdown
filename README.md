# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/Kehlani/Kehlani"><img src="https://lastfm.freetls.fastly.net/i/u/174s/d6e60f3806046f9b440af1a5a364d70a.png" title="Kehlani - Kehlani"></a> <a href="https://www.last.fm/music/Teddy+Pendergrass/It%27s+Time+For+Love"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ebe7c2a3afa97bfeed23ab24512be5ce.jpg" title="Teddy Pendergrass - It's Time For Love"></a> <a href="https://www.last.fm/music/Luther+Vandross/Give+Me+the+Reason"><img src="https://lastfm.freetls.fastly.net/i/u/174s/e33b871209a22d75795babea12b0a23c.jpg" title="Luther Vandross - Give Me the Reason"></a> <a href="https://www.last.fm/music/Future+Islands/One+Day+%2F+The+Ink+Well"><img src="https://lastfm.freetls.fastly.net/i/u/174s/b4410969c77e3be9417081028eda9fb3.png" title="Future Islands - One Day / The Ink Well"></a> <a href="https://www.last.fm/music/Jukebox+the+Ghost/Everything+Under+the+Sun"><img src="https://lastfm.freetls.fastly.net/i/u/174s/ffc3f8a82f9f4cf5e4df0a78bf40d2da.jpg" title="Jukebox the Ghost - Everything Under the Sun"></a> <a href="https://www.last.fm/music/No+Party+For+Cao+Dong/%E7%93%A6%E5%90%88"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a2e13cf825769421e4d299c9b6c6ebfd.jpg" title="No Party For Cao Dong - 瓦合"></a> <a href="https://www.last.fm/music/Dosh/The+Lost+Take"><img src="https://lastfm.freetls.fastly.net/i/u/174s/af9405e857b5445182d147bdf7fb4320.jpg" title="Dosh - The Lost Take"></a> <a href="https://www.last.fm/music/No+Party+For+Cao+Dong/%E9%86%9C%E5%A5%B4%E5%85%92"><img src="https://lastfm.freetls.fastly.net/i/u/174s/a16553471b8e5083b5d18a88735cbe15.jpg" title="No Party For Cao Dong - 醜奴兒"></a> <a href="https://www.last.fm/music/Slayyyter/WOR$T+GIRL+IN+AMERICA"><img src="https://lastfm.freetls.fastly.net/i/u/174s/d60a52a2d3e8eec7d9df29dc18d16ec2.png" title="Slayyyter - WOR$T GIRL IN AMERICA"></a> <a href="https://www.last.fm/music/Angelo+De+Augustine/Angel+in+Plainclothes"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3d81cafa8e24e3b31b7526e4bbc3e783.jpg" title="Angelo De Augustine - Angel in Plainclothes"></a> </p>

          
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
