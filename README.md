# LiaScript lectures

The website opens a course contents page with links to lectures 01–11.
Each lecture is rendered by the existing LiaScript viewer, with the same
styles, slide controls, tables, images, expandable details, and quizzes.
Lecture 01 contains the existing published material. Lectures 02–11 are
placeholders until you add their content.

## Which file do I edit?

| What you want to change | File to edit |
| --- | --- |
| Lecture 01 content | `docs/lecture01.md` |
| Lecture 02 content | `docs/lecture02.md` |
| Other lecture content | `docs/lecture03.md` through `docs/lecture11.md` |
| Contents page, lecture titles, or availability text | `docs/contents.md` |
| Images | Add files inside `docs/Images/` |
| Viewer setup and which Markdown file opens | `docs/index.html` |

**For normal lecture edits, change the lowercase Markdown file in `docs/`.
You do not need to edit `index.html` or regenerate the JavaScript files.**

The root `Lecture01.md` is now a pointer to the active file. The old
`docs/Lecture01.md` has been renamed to lowercase `docs/lecture01.md`.
The previous root copy, which differed from the published copy, is preserved
in `archive/Lecture01-original-root.md`.

Keep filenames exactly as written. For example, `lecture01.md` and
`Lecture01.md` are different paths on GitHub Pages.

## Upload this update to your existing repository

1. Extract `Liascript-updated.zip`.
2. Remove the old `docs/Lecture01.md` from your existing checkout **before**
   copying the update. This avoids a case-only filename conflict on Windows
   and macOS. Its published content is preserved in the new lowercase file.
   With Git, run `git rm docs/Lecture01.md` before the next step.
3. Copy the **contents of its `Liascript-main` folder** into the root of your
   existing repository, replacing matching files. Do not create a second
   nested `Liascript-main` folder inside your repository.
4. Commit and push the files. For example, from your repository folder:

   ```bash
   git add README.md Lecture01.md docs archive
   git commit -m "Add lecture contents page and editable Markdown lectures"
   git push
   ```

5. In GitHub, open **Settings → Pages**. Select **Deploy from a branch**,
   choose your publishing branch (for example `main`), select **`/docs`**,
   and save. If those settings are already selected, leave them as they are.
6. Wait for the Pages deployment to finish, then open the website homepage.
   It will show the lecture list. Click **Lecture 01** to open the notes.

When Pages publishes your branch's `/docs` folder, later commits to
`docs/lecture01.md` are published automatically. No export/build step is needed.
Changing a file locally only affects the website after you commit and push it.

GitHub reference: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

## Add or update a lecture

1. Edit the matching file, for example `docs/lecture02.md`.
2. Replace its “Coming soon” text with your lecture content.
3. Update that lecture's topic and status in `docs/contents.md`.
4. Commit and push both Markdown files.

Use the same LiaScript syntax as in Lecture 01. The first heading names the
lecture; subsequent headings form its sections/slides. Keep the “All lectures”
link near the top so students can return to the contents page.

Images should use relative paths, for example:

```markdown
![A sensor](Images/sensor.png)
```

Save that image at `docs/Images/sensor.png`. Avoid hard-coded links to a
particular GitHub username, so the site works if the repository is moved.

## Preview locally

From the repository root, run:

```bash
python3 -m http.server 8000 --directory docs
```

Then open http://localhost:8000/ in a browser. Serve the folder over HTTP;
opening `index.html` directly as a local file may prevent Markdown loading.

Example lecture link: `index.html?lecture01.md`. The viewer may expand the
query to the full Markdown URL; this is normal LiaScript behavior.
Existing uppercase links such as `index.html?Lecture01.md` also select the
active lowercase file. A slide hash such as `#2` is retained.
