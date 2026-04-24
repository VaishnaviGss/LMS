# LMS Portal — Review Pages

Manager view of all implemented LMS pages. Open `index.html`. Tab bar switches between Dashboard / Courses / Mentors / Students.

## Structure

- `index.html` — iframe shell. Tab buttons load pages into iframe.
- `lms_dashboard.html` — dashboard page
- `lms_courses.html` — course catalog
- `mentor_management.html` — mentor list/detail
- `student_management.html` — student list/detail
- `originals/` — backups of original files

## Add a new page

1. Create new standalone HTML page (same structure as existing pages).
2. Add button to `index.html` nav tabs: `<button class="tab-btn" onclick="loadPage('newpage.html', this)">Label</button>`
3. Save new page in root directory.

## Deploy to GitHub Pages

1. `git init && git add . && git commit -m "initial"`
2. Push to GitHub repo.
3. Repo → Settings → Pages → Source: `main` branch, `/ (root)`.
4. Visit `https://<user>.github.io/<repo>/`.
