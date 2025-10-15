## Hi 👋 I'm `0xCheckMate`



## 📊 GitHub Quick Stats

* **Top languages & usage**: (Generated dynamically below)
* **Most active repos**: (auto-listed by the small script/workflow included)

### Languages & Stats Cards

![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=YOUR_GITHUB_USERNAME\&layout=compact\&card_width=500)

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=YOUR_GITHUB_USERNAME\&show_icons=true\&count_private=true\&theme=default)

---

## 🔭 Most used / Recent repos

<details>
<summary>Top repositories (auto-generated)</summary>

* Repo 1 — short description
* Repo 2 — short description
* Repo 3 — short description

</details>

---

## 🏆 Trophies & Activity

![GitHub Trophies](https://github-profile-trophy.vercel.app/?username=YOUR_GITHUB_USERNAME\&theme=gruvbox)

![Contribution Graph](https://activity-graph.herokuapp.com/graph?username=YOUR_GITHUB_USERNAME\&area=true\&hide_border=true)

---

## ⚙️ Auto-update workflow (optional)

Add the following GitHub Action file to `.github/workflows/update-readme.yml` in your repo. It will run daily, fetch your top repos and overwrite the placeholder section in the README with an auto-generated list.

```yaml
name: Update README with Top Repos
on:
  schedule:
    - cron: '0 6 * * *'
  workflow_dispatch: {}

jobs:
  update-readme:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Get repos and update README
        uses: actions/github-script@v7
        with:
          script: |
            const username = context.actor || 'YOUR_GITHUB_USERNAME'
            const repos = await github.rest.repos.listForUser({ username, per_page: 100 })
            const filtered = repos.data
              .filter(r => !r.fork)
              .sort((a,b) => b.stargazers_count - a.stargazers_count)
              .slice(0, 8)

            const lines = ['<details>','<summary>Top repositories (auto-generated)</summary>','']
            for (const r of filtered) {
              lines.push(`- [${r.name}](${r.html_url}) — ${r.description || ''}`)
            }
            lines.push('', '</details>')

            const fs = require('fs')
            const readme = fs.readFileSync('README.md', 'utf8')
            const start = '<!-- START_TOP_REPOS -->'
            const end = '<!-- END_TOP_REPOS -->'
            const before = readme.split(start)[0]
            const after = readme.split(end)[1] || ''
            const newReadme = before + start + '\n' + lines.join('\n') + '\n' + end + after
            fs.writeFileSync('README.md', newReadme)

      - name: Commit & push
        uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit_message: "chore: update README (top repos)"
          file_pattern: README.md
```

> Note: The workflow above uses `actions/github-script` which uses the `GITHUB_TOKEN` and fetches public repos. If you want private repos included, adjust permissions and consider using a Personal Access Token stored as a secret.

---


<details>
<summary>Top repositories (auto-generated)</summary>

* (this will be replaced by the action)

</details>


---