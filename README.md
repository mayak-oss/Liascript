# LiaScript lectures for GitLab Pages

The homepage opens a contents page with links to lectures 01–11. All active
lecture Markdown files are in one folder: **`docs/lectures/`**.
Lecture 01 keeps the existing published material and LiaScript styling,
images, slide controls, tables, expandable details, and quizzes. Lectures
02–11 are placeholders until you add their content.

## Project folders

| Path | Purpose |
| --- | --- |
| `docs/lectures/lecture01.md` | Edit Lecture 01 here |
| `docs/lectures/lecture02.md` through `lecture11.md` | Edit the other lectures here |
| `docs/contents.md` | Edit the contents list, topics, and availability text |
| `docs/Images/` | Shared lecture images |
| `docs/index.html` | Existing LiaScript viewer and course loader |
| `.gitlab-ci.yml` | GitLab Pages deployment configuration |
| `archive/Lecture01-original-root.md` | Preserved original root copy; not published |

**For normal lecture edits, change the Markdown file in `docs/lectures/`.
You do not need to edit `index.html` or regenerate JavaScript files.**
The root `Lecture01.md` is a pointer to the active file.

## Apply this update from Mac Terminal

Download `Liascript-lectures-folder.zip` into your Downloads folder.
Use a clean checkout of your **GitLab** project. Replace the example path
below with your actual repository folder. If you have local edits, commit
them before pulling or applying an update; the package replaces matching files.

```bash
cd /path/to/your/Liascript
git remote -v
git pull --ff-only

unzip ~/Downloads/Liascript-lectures-folder.zip -d ~/Downloads/Liascript-folder-update

git rm --ignore-unmatch -- docs/Lecture01.md docs/lecture{01..11}.md

rsync -av --exclude='.gitlab-ci.yml' ~/Downloads/Liascript-folder-update/Liascript-main/ ./

if [ ! -f .gitlab-ci.yml ]; then
  cp ~/Downloads/Liascript-folder-update/Liascript-main/.gitlab-ci.yml .gitlab-ci.yml
fi

git add README.md Lecture01.md docs archive .gitlab-ci.yml
git status
git commit -m "Organize lectures in one folder and configure GitLab Pages"
git push
```

Check that `git remote -v` shows your intended GitLab repository. A GitHub
remote does not push directly to GitLab. If you have not cloned your GitLab
project yet, copy its clone URL from GitLab's Code menu and run
`git clone YOUR_GITLAB_CLONE_URL`, then enter that checkout.

The commands remove only the old flat lecture paths before adding the new
folder. This also handles the uppercase filename in the original upload on
macOS. An existing `.gitlab-ci.yml` is preserved by the copy commands.
If any command fails, stop and resolve that error before continuing.

## GitLab Pages deployment

The included `.gitlab-ci.yml` is for GitLab 17.9 or later and publishes the
entire `docs` folder from the project's default branch. There is no lecture
export or site build step: the browser renders the Markdown using the
bundled LiaScript viewer.

If your repository already has `.gitlab-ci.yml`, keep its other jobs and
make its Pages job publish `docs`. The included file shows the configuration.
If that existing job already publishes `docs` (or copies all of `docs/` into
its published directory), no deployment change is needed. Include the full
folder, including `lectures/`, `Images/`, and the bundled viewer assets.

After a push to the default branch, check **Build → Pipelines**. When the
Pages deployment succeeds, find your website under **Deploy → Pages**.
If you work on another branch, merge it into the default branch to publish.
You do not use GitHub's branch-and-folder Pages settings for GitLab.

GitLab reference: https://docs.gitlab.com/user/project/pages/introduction/#customize-the-default-folder

## Edit and publish a lecture

From the repository root, open Lecture 01 in TextEdit:

```bash
git pull --ff-only
open -e docs/lectures/lecture01.md
```

Save your changes, then run:

```bash
git add docs/lectures/lecture01.md
git commit -m "Update lecture 01"
git push
```

Use the same LiaScript syntax as in Lecture 01. Keep filenames lowercase,
with a leading zero for lectures 01–09. Keep the “All lectures” link near
the beginning of each lecture so students can return to the list.

## Images and return links

Lectures are one folder below the shared Images folder. For an image saved
at `docs/Images/sensor.png`, use this inside a lecture:

```markdown
![A sensor](../Images/sensor.png)
```

An HTML image uses the same path:

```html
<img src="../Images/sensor.png" alt="A sensor">
```

The return link inside a lecture is:

```html
<a href="../index.html?contents.md" target="_self">← All lectures</a>
```

In `docs/contents.md`, which is one level higher, the lecture link is:

```html
<a href="index.html?lectures/lecture01.md" target="_self">Lecture 01</a>
```

## Add Lecture 12 later

1. Copy a placeholder to `docs/lectures/lecture12.md`.
2. Change the heading and replace the placeholder text with your content.
3. Add a row in `docs/contents.md`, linking to
   `index.html?lectures/lecture12.md`.
4. Commit and push both Markdown files.

No `index.html` edit is required for more lectures. You can also change the
titles, topics, and status of lectures 02–11 in the contents table as you
write them.

## Local preview

From the repository root:

```bash
python3 -m http.server 8000 --directory docs
```

Open http://localhost:8000/ in a browser. Use HTTP rather than opening the
HTML directly from disk, because loading Markdown requires browser fetches.

New lecture link: `index.html?lectures/lecture01.md`.
Legacy links such as `index.html?Lecture01.md` and
`index.html?lecture01.md` also select `docs/lectures/lecture01.md`.
Slide hashes such as `#2` are retained. LiaScript may expand the query to
the full Markdown URL; that is normal.
