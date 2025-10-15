# Hi 👋 I’m 0xCheckMate

I’m a cybersecurity / developer enthusiast working on payload execution, exploits, and exploring Windows internals.  
Currently building tools and proof-of-concepts; writeups coming soon.

---

## 📊 GitHub Stats & Languages

![0xCheckMate’s GitHub Stats](https://github-readme-stats.vercel.app/api?username=0xCheckMate&show_icons=true&count_private=true&theme=default)  
![0xCheckMate’s Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=0xCheckMate&layout=compact&theme=default)

---

## 🧰 Projects & Repositories

Here are some of my public repos (auto-updated via workflow below):

<!-- START_TOP_REPOS -->
<details>
<summary>Top repositories (auto-generated)</summary>

- [Checkmate](https://github.com/0xCheckMate/Checkmate) — payload execution by Fake Windows SmartScreen & privilege escalation POC

</details>
<!-- END_TOP_REPOS -->

---

## 🏆 Trophies & Activity

![GitHub Trophies](https://github-profile-trophy.vercel.app/?username=0xCheckMate&theme=gruvbox)  
![Contribution Graph](https://activity-graph.herokuapp.com/graph?username=0xCheckMate&area=true&hide_border=true)

---

## ⚙️ Auto-Update Workflow for Projects List

You can add the following GitHub Action in `.github/workflows/update-readme.yml` to auto-update the **Projects** section. It fetches your public non-fork repos and injects the list between the `<!-- START_TOP_REPOS -->` and `<!-- END_TOP_REPOS -->` markers in your README.

```yaml
name: Update README with Top Repos
on:
  schedule:
    - cron: '0 6 * * *'  # daily at 06:00 UTC
  workflow_dispatch: {}

jobs:
  update-readme:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Get top repos & update README
        uses: actions/github-script@v7
        with:
          script: |
            const username = '0xCheckMate'
            const repos = await github.rest.repos.listForUser({
              username,
              per_page: 100
            })
            const filtered = repos.data
              .filter(r => !r.fork)
              .sort((a, b) => b.stargazers_count - a.stargazers_count)
              .slice(0, 8)

            const lines = ['<details>', '<summary>Top repositories (auto-generated)</summary>', '']
            for (const r of filtered) {
              lines.push(`- [${r.name}](${r.html_url}) — ${r.description || ''}`)
            }
            lines.push('', '</details>')

            const fs = require('fs')
            const readme = fs.readFileSync('README.md', 'utf8')
            const startTag = '<!-- START_TOP_REPOS -->'
            const endTag = '<!-- END_TOP_REPOS -->'
            const before = readme.split(startTag)[0]
            const after = readme.split(endTag)[1] || ''
            const newReadme = before + startTag + '\n' + lines.join('\n') + '\n' + endTag + after
            fs.writeFileSync('README.md', newReadme)

      - name: Commit & push
        uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit_message: "chore: update README with top repos"
          file_pattern: README.md
