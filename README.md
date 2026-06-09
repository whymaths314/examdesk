# ExamDesk — Complete User Guide

---

## What is ExamDesk?

ExamDesk is a **single-file exam platform** (`index.html`) that runs entirely in any web browser. It supports:
- ✅ Multiple Choice Questions (MCQ) with auto-grading
- ✅ Written / Essay questions with keyword-based auto-scoring
- ✅ Multiple exams with different settings
- ✅ Student name + ID tracking
- ✅ Countdown timer per exam
- ✅ Admin panel with password protection
- ✅ View all student responses
- ✅ Export responses to CSV (for Excel/Google Sheets)
- ✅ Backup and restore questions as JSON
- ✅ Works 100% offline — no internet needed after first load

---

## 1. How to Open & Use It (Locally)

1. Find the file `index.html` in your downloads.
2. Double-click it — it opens in your browser (Chrome, Firefox, Edge, Safari all work).
3. That's it. No installation. No server needed.

> **Tip:** Your data (exams, questions, responses) is saved in your **browser's localStorage**. It persists after closing the tab. It is tied to that browser on that computer.

---

## 2. How to Host It Online (Free Options)

### Option A — GitHub Pages (Recommended, Free)

1. Create a free account at https://github.com
2. Click **New Repository** → name it `examdesk` → set it to **Public**
3. Upload `index.html` to the repository
4. Go to **Settings → Pages** → Source: `main` branch → root folder → Save
5. Your exam will be live at:
   `https://YOUR-USERNAME.github.io/examdesk`
6. Share that link with your students.

**Update the site:** Just upload a new `index.html` and GitHub Pages refreshes in ~1 minute.

---

### Option B — Netlify Drop (Easiest, Free)

1. Go to https://netlify.com → sign up free
2. Drag and drop your `index.html` onto the Netlify dashboard
3. You get a URL like `https://random-name.netlify.app` instantly
4. You can set a custom name in settings.

---

### Option C — Any Web Hosting

Upload `index.html` to any hosting provider (Hostinger, cPanel, InfinityFree, etc.) via FTP or their file manager. Access at your domain.

---

### Option D — Google Drive (Share with specific people)

1. Upload `index.html` to Google Drive
2. Right-click → Share → "Anyone with the link"
3. Use a service like https://sites.google.com to embed it, or share directly.
> Note: Google Drive doesn't directly serve HTML. Use Netlify or GitHub Pages instead for full functionality.

---

## 3. Admin Panel

**Default password:** `admin123`

To access:
1. Click **Admin** in the top navigation
2. Enter the password

### Changing the Admin Password
- Inside the Admin Panel → **Settings** tab → enter a new password → Save Settings
- Or open `index.html` in a text editor and find the line:
  ```
  const ADMIN_PASSWORD = "admin123";
  ```
  Change `admin123` to whatever you want, save the file.

---

## 4. Creating an Exam

1. Go to **Admin → Exams** tab
2. Fill in:
   - **Exam Title** — e.g. "Chapter 5 Quiz"
   - **Subject** — e.g. "Physics"
   - **Description** — shown to students before they start
   - **Time Limit** — in minutes. Set `0` for no time limit.
   - **Passing Score** — percentage needed to pass (e.g. 60)
   - **Status** — `Open` (students can see/take it) or `Closed`
   - **Shuffle Questions** — randomize order for each student
3. Click **Save Exam**

---

## 5. Adding Questions

### Adding MCQ Questions

1. Go to **Admin → Questions** tab
2. Select the **Exam** from the dropdown
3. Set **Type** to `Multiple Choice (MCQ)`
4. Type the **question text**
5. Fill in all option fields (A, B, C, D...)
6. Click the **radio button (●)** next to the correct answer
7. Set **Points** (how many points this question is worth)
8. Click **Save Question**

### Adding Written/Essay Questions

1. Same as above but set **Type** to `Written / Essay`
2. Optionally enter **Expected Keywords** (comma-separated)
   - These are used for automatic partial scoring
   - Example: `photosynthesis, chlorophyll, sunlight, ATP`
3. Set **Minimum Words** expected
4. Click **Save Question**

---

## 6. Upgrading / Editing MCQ Questions

### Method A — Through the Admin Panel (Recommended)
1. Go to **Admin → Questions**
2. Filter by exam using the dropdown
3. Find the question → click **Edit**
4. Make your changes
5. Click **Save Question**

### Method B — Edit the HTML File Directly (for bulk changes)

Open `index.html` in any text editor (Notepad, VS Code, TextEdit).

Find the `SAMPLE_QUESTIONS` array near the top:

```javascript
const SAMPLE_QUESTIONS = [
  {
    id: "q-001",           // Unique ID — never duplicate
    examId: "exam-001",    // Must match an exam's id
    type: "mcq",           // "mcq" or "written"
    text: "Your question here?",
    points: 2,             // How many points
    options: [             // MCQ options
      "Option A",
      "Option B",
      "Option C",
      "Option D"
    ],
    correct: 0,            // 0 = A, 1 = B, 2 = C, 3 = D
    keywords: "",          // Leave blank for MCQ
    minWords: 0            // Leave 0 for MCQ
  },
  // ... more questions
];
```

**Important:** After editing the file, open it in a **new browser tab** (or clear localStorage) for the new questions to load as defaults.

> **Note:** Once you've used the admin panel, data is stored in localStorage and the SAMPLE data is ignored. Use **Export → Download questions.json** to back up and **Import** to restore.

---

## 7. Where Responses Are Saved

All student responses are saved in your **browser's localStorage**. This means:

| ✅ Works | ❌ Doesn't work |
|---|---|
| Responses persist after closing tab | Responses don't sync across devices |
| No server or database needed | Clearing browser data = losing responses |
| All data stays private on your machine | Students can't take exam offline on their device and sync to yours |

### How to Access Responses

1. Admin Panel → **Responses** tab
2. Filter by exam
3. See each student's name, score, and individual answers

### How to Save Responses Permanently

- **Export as CSV** (Admin → Export → Download CSV)
  - Opens in Excel or Google Sheets
  - Contains: Student name, ID, exam, score, each answer

- **Back up regularly** — export CSV after each exam session

---

## 8. Memory & Storage Management

### How much can it store?
localStorage holds up to **5–10 MB** per browser. That's roughly:
- ~500 questions with long text
- ~1,000+ student responses

### How to free up space
1. Export responses to CSV first
2. Go to **Admin → Responses** → click **Clear All Responses**

### How to fully reset
Admin → Settings → **Delete Everything & Reset**

### Moving data to a new computer
1. On the old computer: **Admin → Export → Download questions.json**
2. On the new computer: Open `index.html` → Admin → **Export → Import** → upload the JSON file

---

## 9. Grading

### MCQ Questions
Graded **automatically** — 100% accurate.

### Written Questions
Written answers are graded with a **keyword scoring system**:
- You define expected keywords when creating the question
- If a student's answer contains those keywords, they receive partial credit (up to 60% of points automatically)
- The remaining points must be reviewed manually in the **Responses** tab

To fully manually grade written responses:
1. Go to **Admin → Responses**
2. Read each student's written answer
3. Record your manual grade in your CSV export

---

## 10. Sharing With Students

Once hosted (GitHub Pages, Netlify, etc.):
1. Share the URL with students via WhatsApp, email, or classroom system
2. Students go to the link → click **Student** tab → enter their name → select exam → start
3. They see results immediately after submitting
4. You see their responses in the Admin panel

---

## 11. Tips & Best Practices

- **Close the exam** when done: Admin → Exams → Edit → Status = Closed. This prevents new submissions.
- **Export CSV right after each exam** — don't rely solely on localStorage.
- **Use Student ID field** to match responses with your class list.
- **Set time limits** to prevent students lingering too long.
- **Use Shuffle** to reduce cheating (each student sees a different order).
- **Back up questions.json weekly** so you don't have to re-enter everything.

---

## 12. File Structure

```
index.html          ← The entire platform in one file
examdesk-backup.json ← Your exported question bank (keep this safe!)
responses.csv        ← Exported student responses
```

---

## 13. Technical Notes

- **No server required** — pure HTML/CSS/JavaScript
- **No cookies** — uses localStorage only
- **Works offline** after first load (fonts load from Google Fonts on first open)
- **Responsive** — works on phones, tablets, desktops
- **Browser support** — Chrome, Firefox, Safari, Edge (any modern browser)

---

*ExamDesk — Built for teachers who just want it to work.*
