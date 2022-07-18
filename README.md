# website

![Deploy to GitHub Pages](https://github.com/h1g0/website/workflows/Deploy%20to%20GitHub%20Pages/badge.svg)

source code for [my website](https://github.com/h1g0/h1g0.github.io/).

## how to post articles

1. Clone this repository. e.g. `git clone git@github.com:h1g0/website.git`
2. Set up [Hugo](https://gohugo.io) if you want to use functions such as preview, etc.
3. create new markdown file in `content`directory.
   - If you have Hugo set up, go to the repository dir and `hugo new content/blog/yyyy/mm/titile-of-article.md`.
4. Write the article.
5. Commit and push.
6. GitHub Actions will automatically run hugo and deploy to the website.
