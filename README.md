# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><a href="https://www.last.fm/music/LEENALCHI/Here+Comes+That+Crow"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3e4544374155a827574891bd66e56670.jpg" title="LEENALCHI - Here Comes That Crow"></a> <a href="https://www.last.fm/music/Clarence+Clarity/No+Now"><img src="https://lastfm.freetls.fastly.net/i/u/174s/c3334f01531970ce1cc60de7c139b70d.jpg" title="Clarence Clarity - No Now"></a> <a href="https://www.last.fm/music/Joshua+Burnside/It%27s+Not+Going+To+Be+Okay"><img src="https://lastfm.freetls.fastly.net/i/u/174s/bdfcbfc0625d6d949cabc3fa80e00aea.jpg" title="Joshua Burnside - It's Not Going To Be Okay"></a> <a href="https://www.last.fm/music/CFCF/L.U.V."><img src="https://lastfm.freetls.fastly.net/i/u/174s/17a878637b55cb707ac72380b17230c7.jpg" title="CFCF - L.U.V."></a> <a href="https://www.last.fm/music/indigo+la+End/%E6%BA%80%E3%81%A1%E3%81%9F%E7%B4%AB"><img src="https://lastfm.freetls.fastly.net/i/u/174s/fd465d29dcb2b942b9e8591ff95e4d94.jpg" title="indigo la End - 満ちた紫"></a> <a href="https://www.last.fm/music/Andris+Mattson/the+understory"><img src="https://lastfm.freetls.fastly.net/i/u/174s/fba1ca10af3cf818bc73674641fdeb6d.jpg" title="Andris Mattson - the understory"></a> <a href="https://www.last.fm/music/Judeline/BODHIRIA"><img src="https://lastfm.freetls.fastly.net/i/u/174s/7408ed008b4d9ff47a35d2d7c628761a.jpg" title="Judeline - BODHIRIA"></a> <a href="https://www.last.fm/music/Vetle+N%C3%A6r%C3%B8/From+Moments"><img src="https://lastfm.freetls.fastly.net/i/u/174s/0bfbe42ef291c217f464e05d810764f8.jpg" title="Vetle Nærø - From Moments"></a> <a href="https://www.last.fm/music/YOASOBI/THE+BOOK+for,"><img src="https://lastfm.freetls.fastly.net/i/u/174s/3e3329379735b86d69f45fd201968479.jpg" title="YOASOBI - THE BOOK for,"></a> <a href="https://www.last.fm/music/Ahub/UMANITY"><img src="https://lastfm.freetls.fastly.net/i/u/174s/69be93db5a1b4dce266ac77dd89e3709.jpg" title="Ahub - UMANITY"></a> </p>

          
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
