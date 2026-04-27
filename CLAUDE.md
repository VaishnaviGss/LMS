# Admin LMS Portal Documentation

## 1. Project Overview

**Admin Learning Management System (LMS) Portal** — Organization-facing admin dashboard for managing educational content, students, mentors, finances, and institution settings.

**Purpose**: Centralized control panel for institutional admins to:
- Create & manage courses with levels, topics, and pricing
- Enroll students, track attendance, manage subscriptions
- Assign mentors, manage commissions, track payouts
- Monitor financial transactions (invoices, payments)
- Schedule and track class sessions
- View analytics and real-time activity

**Architecture**: Single-page iframe-based shell with modular content pages. Sidebar navigation loaded from `index.html` with nested pages rendering inside iframe. Pure HTML/CSS/JS (no external dependencies).

**Viewport**: Fixed 1440px width design, optimized for desktop admin use.

---

## 2. File Structure

### Main Entry Point
- **`index.html`** — Shell container with sidebar navigation + iframe frame for page switching

### Core Admin Pages (loaded via iframe)
- **`dashboard.html`** — Admin overview: KPIs, recent activity, upcoming sessions, quick links
- **`courses_page.html`** — Course list/grid, course detail view, level/topic management
- **`student_management.html`** — Student list, enrollment, subscription status, attendance tracking
- **`mentor_management.html`** — Mentor profiles, commission rates, payout status, course assignments
- **`class_sessions.html`** — Schedule, live sessions, session history
- **`attendance.html`** — Daily/weekly attendance tracking, reporting
- **`levels.html`** — Topics & assets (course content hierarchy)
- **`payouts.html`** — Mentor payout tracking, payment status
- **`settings.html`** — Organization settings, user preferences

### Support Files
- **`originals/`** — Backup copies of initial versions (reference only)
- **`.vscode/`** — VS Code config (local development)

---

## 3. Feature Breakdown by Page

### Dashboard (`dashboard.html`)
**Key Metrics (KPI Grid)**
- Total Students: 247 (with weekly delta)
- Active Courses: 12 (monthly delta)
- Revenue (April): ₹2.4L (% change)
- Pending Invoices: 3 (outstanding amount)

**Hero Banner**
- Welcome message + current period stats
- Quick action buttons: Enroll Student, Generate Invoice

**Recent Activity Card**
- Live feed of system events (enrollments, payments, attendance marks, subscription changes)
- Timestamp + action type + actor name

**Today's Class Sessions**
- Table: Time | Student | Course | Mentor | Status (Completed/Scheduled) | Attendance
- Status pills: completed (blue), scheduled (teal)

**Bottom Quick Links (4-card row)**
- Pending Payouts → links to Payouts page
- Present Today → links to Attendance
- Course Levels → links to Levels
- Sessions Today → links to Class Sessions

### Courses (`courses_page.html`)
**List View (Grid Layout)**
- Course cards: thumbnail (gradient bg + icon) | name | description | students count | levels count | total classes | pricing | status badge
- Status badges: Active (green), Inactive (red)
- Hover actions: View | Edit | Deactivate

**Filters & Search**
- Search by name/topic
- Filter by Status (All/Active/Inactive)
- Filter by Pricing Model (All/Per Month/Per Class/Per 10 Classes)
- Count tag showing filtered results

**Detail View (flip to detail screen)**
- Back button + header (course name, color icon, metadata: status, pricing, course ID)
- Stats strip: Students enrolled | Levels | Topics | Revenue

**Tabs in Detail View**
1. **Enrolled Students**
   - Table: Student | Phone | Subscription Status | Classes Taken | Attendance % | Actions
   - Status pills: active, paused, completed, cancelled
   - Attendance bar chart (colored by threshold: 80%+ green, else amber)
   - Inline status change dropdown

2. **Levels & Topics**
   - Collapsible level rows (numbered 1-3, color-coded)
   - Each level shows: name | topic count | click to expand
   - Expanded topics show: topic name | asset count | view/edit actions
   - Add Topic button in level row

3. **Assigned Mentors**
   - Mentor cards: avatar | name | courses assigned | commission rate | status badge
   - Actions: Edit | Remove mentor

4. **Pricing History**
   - Table: Model | Amount | Effective From | Effective To | Status
   - Status: Current (active) | Expired (inactive)

**Modals**
- **Create New Course**: name, description, pricing model (dropdown), price amount, classes count (conditional), active toggle
- **Add Student to Course**: searchable dropdown (student name), status, billing type, enrollment date
- **Add Level**: name, description, sequence order, duration in weeks
- **Add Topic**: name, description, asset type (Video/Audio/PDF), access level
- **Add Mentor to Course**: searchable dropdown (mentor), commission %, assign from date

### Students (`student_management.html`)
**List View**
- Table: Student | Email | Phone | Status | Courses Enrolled | Outstanding Balance | Actions
- Status pills: active, paused, completed, cancelled
- Sortable columns

**Filters**
- Search by name/email
- Status filter
- Course enrollment filter

**Student Detail View**
- Profile: avatar | name | email | phone | enrollment date | status
- Enrollment summary: active/paused/completed courses
- Attendance overview (overall %)
- Subscription management: list of courses with status, classes taken, attendance

**Actions**
- Change subscription status
- Pause/resume course
- View attendance detail
- Send notification
- View invoices/payments

### Mentors (`mentor_management.html`)
**List View**
- Cards/Table: Mentor avatar | name | courses assigned | pending payout | commission rate | status
- Status: Active | On Leave | Inactive

**Mentor Detail**
- Profile: name, email, phone, specialization, bio
- Assigned courses: list with student count, classes done, pending commission
- Payout history: table of amounts, dates, status
- Performance metrics: avg rating, completion rate, student satisfaction

**Management**
- Edit mentor profile
- Assign/remove courses
- Adjust commission rates
- Mark on leave/inactive
- Trigger payout

### Class Sessions (`class_sessions.html`)
**Schedule View**
- Calendar grid showing scheduled sessions
- Time slots: date | start/end time | student | mentor | course | status

**Session Statuses**
- Scheduled (blue)
- In Progress (amber)
- Completed (green)
- Cancelled (red)

**Actions**
- Schedule new session (date picker, time, student, mentor, course)
- Edit session details
- Mark session complete
- Collect attendance

### Attendance (`attendance.html`)
**Tracking Interface**
- Date picker (view by day/week/month)
- Table: Student | Course | Mentor | Time In | Time Out | Status
- Status badges: Present (green), Absent (red), Late (amber)
- Mark attendance: quick toggle buttons

**Reports**
- Attendance %  by student
- Daily/weekly summary
- Delinquent student alerts
- Export options

### Levels (`levels.html`)
**Content Hierarchy**
- Expandable list: Level 1 | Level 2 | Level 3 (color-coded)
- Topics under each level
- Assets under each topic

**CRUD Operations**
- Create level with name, description, duration
- Add topics: name, description, asset type, access level
- Upload/link assets: video, audio, PDF, documents
- Set access restrictions: View Only, Download Allowed, Restricted

### Payouts (`payouts.html`)
**Payout Management**
- Pending payouts: list of mentors waiting payment
- Amount, date due, payment method
- Mark as paid, request bank details

**History**
- Completed payouts: date, amount, mentor, transaction ID
- Filter by date range, mentor, status

### Settings (`settings.html`)
**Organization Settings**
- Institution name, logo, contact info
- Default commission rates
- Payment methods (bank details, payment gateway)

**System Settings**
- Admin user management: add/remove admins, assign roles
- Notification preferences
- Export/backup data

---

## 4. Data Structures

### Course Object
```javascript
{
  id: 'CRS001',
  name: 'Mathematics — Grade 10',
  color: '#7c3aed',  // for UI cards
  status: 'active' | 'inactive',
  desc: 'description',
  pricing: '₹3,500/mo',  // display string
  model: 'Per Month' | 'Per Class' | 'Per 10 Classes',
  revenue: '₹2.1L',
  totalClasses: 248,
  students: [{ name, phone, subStatus, classes, att }],
  levels: [{ name, color, topics: [{ name, assets }] }],
  mentors: [{ name, av, color, commission, courses }],
  pricing_history: [{ model, amount, from, to, current }]
}
```

### Student Object
```javascript
{
  id: 'STU001',
  name: 'Priya Sharma',
  email: 'priya@email.com',
  phone: '98765 43210',
  status: 'active' | 'paused' | 'completed' | 'cancelled',
  enrollmentDate: '2026-01-15',
  coursesEnrolled: [{ courseId, status, classesAttended, totalClasses, att% }],
  outstandingBalance: ₹0,
  overallAttendance: 87
}
```

### Mentor Object
```javascript
{
  id: 'MEN001',
  name: 'Anand Kumar',
  email: 'anand@email.com',
  phone: '91234 56789',
  specialization: 'Mathematics',
  status: 'active' | 'on_leave' | 'inactive',
  coursesAssigned: [{ courseId, courseName, studentCount, classesDone, commission% }],
  defaultCommission: 30,
  pendingPayout: ₹12450,
  payoutHistory: [{ amount, date, status, txnId }]
}
```

### Invoice Object
```javascript
{
  id: 'INV-0241',
  studentId: 'STU001',
  studentName: 'Priya Sharma',
  courseId: 'CRS001',
  amount: ₹6400,
  dueDate: '2026-05-01',
  paidDate: '2026-04-20',
  status: 'paid' | 'pending' | 'overdue' | 'cancelled',
  items: [{ description, amount, quantity }]
}
```

### Class Session Object
```javascript
{
  id: 'SES001',
  courseId: 'CRS001',
  mentorId: 'MEN001',
  studentId: 'STU001',
  scheduledDate: '2026-04-27',
  startTime: '09:00',
  endTime: '10:00',
  status: 'scheduled' | 'in_progress' | 'completed' | 'cancelled',
  attendance: 'present' | 'absent' | 'late',
  topicsCovered: ['topic1', 'topic2'],
  notes: 'optional notes'
}
```

### Attendance Record
```javascript
{
  id: 'ATT001',
  studentId: 'STU001',
  courseId: 'CRS001',
  date: '2026-04-27',
  sessionId: 'SES001',
  timeIn: '09:05',
  timeOut: '10:00',
  status: 'present' | 'absent' | 'late',
  markedBy: 'MEN001',
  timestamp: '2026-04-27T10:00:00Z'
}
```

---

## 5. Design System

### Color Palette
**Primary**
- `--accent: #7c3aed` (purple, main brand)
- `--accent2: #a78bfa` (lighter purple)
- `--accent-light: #ede9fe` (very light purple background)

**Status Colors**
- `--green: #059669`, `--green-bg: #ecfdf5` (active/completed)
- `--red: #dc2626`, `--red-bg: #fef2f2` (error/cancelled)
- `--amber: #d97706`, `--amber-bg: #fffbeb` (warning/pending)
- `--blue: #2563eb`, `--blue-bg: #eff6ff` (info/scheduled)
- `--teal: #0891b2`, `--teal-bg: #ecfeff` (secondary info)

**Neutrals**
- `--bg: #f4f3f0` or `#f0f2f8` (page background)
- `--surface: #ffffff` (cards, modals)
- `--text: #1a1a2e` (primary text)
- `--muted: #6b7280` (secondary text)
- `--border: rgba(0,0,0,0.07)` (dividers)

**Sidebar**
- `--sidebar-bg: #1a0533` or `#16012e` (dark purple)
- `--sidebar-hover: rgba(255,255,255,0.06-0.07)` (hover state)
- `--sidebar-active: rgba(167,139,250,0.15-0.22)` (active item bg)

### Typography
**Fonts**
- Display/Headlines: `'Sora', sans-serif` (weights: 400, 600, 700, 800)
- Body/UI: `'Plus Jakarta Sans', sans-serif` (weights: 300, 400, 500, 600, 700)

**Font Sizes**
- Page title: 15-16px, weight 700
- Card titles: 13-13.5px, weight 700
- Body text: 12-14px, weight 500
- Labels: 10-11.5px, weight 600, uppercase with letter-spacing
- Metadata: 10-11px, color muted

### Spacing & Radius
- `--radius: 12-14px` (cards, modals)
- `--radius-sm: 8-10px` (buttons, inputs)
- `--radius-xs: 6-7px` (tags, small elements)
- Padding: 16-20px (cards), 12-14px (modals), 9-12px (items)
- Gap: 8-16px (flex layouts)

### Shadows
- `--shadow-sm: 0 1px 3px rgba(0,0,0,0.06)` (subtle)
- `--shadow: 0 4px 12px rgba(0,0,0,0.06)` (standard cards)
- `--shadow-lg: 0 12px 32px rgba(0,0,0,0.1)` (modals, overlays)

### Components

**Buttons**
- `.btn-primary`: gradient purple, white text, shadow, hover lift
- `.btn-blue`: solid blue, white text, shadow
- `.btn-ghost`: transparent, border, hover bg
- `.btn-back`: blue-bg with teal border, icon + text
- Sizes: large (36-40px H), standard (32-36px), small (28-30px)

**Pills/Badges**
- Status indicators: colored bg + text (active, inactive, pending, paid, completed, paused, cancelled)
- Dot prefix (5px circle)
- Sizes: 11px font, 3-10px padding

**Cards**
- Border: 1.5px solid `--border`
- Radius: 12-14px
- Shadow: standard shadow
- Header with title + actions (flex space-between)
- Body with padding

**Tables**
- Header bg: `--bg`, uppercase small labels, 0.05em letter-spacing
- Rows: hover highlight (`#f0f7ff` or similar)
- Clickable rows trigger detail views
- Status pills/badges in cells

**Modals**
- Backdrop: `rgba(0,0,0,0.42)` with `backdrop-filter: blur(4px)`
- Box: 500px wide, rounded 16px
- Animation: pop-in (0.22s, scale + translateY)
- Header with title + close button
- Body with form rows
- Footer with Cancel/Action buttons

**Forms**
- Input height: 35px
- Border: 1.5px solid `--border`
- Focus: blue border + 3px shadow outline
- Label: 11.5px, uppercase, font-weight 700
- Toggles: slide animation, 36px wide × 20px tall

**Dropdowns (Searchable)**
- Trigger: displays selected value + chevron
- Panel: positioned below, shadow, max-height with scroll
- Search field: 30px, positioned inside panel
- Options: 12px text, hover highlight, selected state (blue text + bg)
- Mini avatars: 26px circles with initials

---

## 6. Navigation & Layout

### Layout Structure (All Pages)
```
┌─ index.html (shell with sidebar + iframe) ─────────────────┐
│                                                              │
│ ┌──────────────────┬──────────────────────────────────────┐│
│ │    SIDEBAR       │  TOPBAR + IFRAME CONTENT             ││
│ │    (240-248px)   │                                      ││
│ │                  │  ┌──────────────────────────────────┐││
│ │  • Logo          │  │ Title | Search | Icons | User    │││
│ │  • Nav items     │  ├──────────────────────────────────┤││
│ │  • Badges        │  │                                  │││
│ │  • User card     │  │ PAGE CONTENT (LIST/GRID/DETAIL)  │││
│ │                  │  │ (rendered by page.html)          │││
│ │                  │  │                                  │││
│ └──────────────────┴──────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────┘
```

### Sidebar Navigation
**Sections**
1. **MAIN** — Dashboard, Courses, Students, Mentors, Class Sessions, Attendance
2. **FINANCE** — Invoices (with red badge for count), Payouts, Wallets
3. **CONTENT** — Topics & Assets (Levels page)
4. **SYSTEM** — Settings

**Active State**
- Nav items: lighter purple background + purple left border (2px)
- Color shift: rgba(255,255,255,0.5) → #fff
- Hovered (non-active): +7% white background

**Badges**
- Purple badges (default): count of items (e.g., 12 courses, 247 students)
- Red badges: alerts/pending (e.g., 3 pending invoices)

### Topbar (Per Page)
**Left Side**
- Page title (15-16px, bold, Sora font)
- Breadcrumb (11px, muted color, e.g., "Sudarshini Institute · 6 courses")
- Search input (340px max width, rounded 20px, with icon)

**Right Side**
- Notification bell icon (with red dot if unread)
- User profile: avatar + name + role

### Content Area
**List View Pattern**
- Toolbar: filters (select dropdowns) + count tag + action button (New/Add)
- Grid/Table area: scrollable main content (custom scrollbar: 4px wide)

**Detail View Pattern**
- Back button → "Back to List"
- Header card: icon | title + metadata
- Tabs for sub-sections
- Tab panels: cards with nested content

### Modals
**Trigger**: Click buttons like "New Course", "Add Student", etc.
**Behavior**:
- Click outside to close (backdrop click)
- Close button (X) in header
- Form fields with labels
- Footer: Cancel | Action buttons (right-aligned)

---

## 7. Key Behaviors

### List/Detail Toggle (Courses, Students, etc.)
- List view renders grid or table
- Click row/card → switches to detail screen
- Detail shows related data in tabs
- Back button returns to list
- Search/filter preserved on return

### Modals (Create/Edit)
- **Open**: Button click adds `.open` class to `.mo` element
- **Close**: Click outside backdrop or X button removes `.open` class
- **Form Submission**: Validates, submits (currently client-side only), closes modal
- **Reset**: Form clears on close (todo: actual backend persistence)

### Filtering & Sorting
- **Dropdowns**: Status, Pricing Model, Course Type — filter immediate
- **Search Input**: On input event, filters list client-side (name/description matching)
- **Count Tag**: Updates to show filtered result count
- **Sort**: Columns clickable (todo: implement sort indicators)

### Status Dropdowns (Inline)
- Click pencil/status icon in table row
- Dropdown menu appears (position: absolute, right-aligned)
- Select new status → updates cell pill immediately
- Menu closes
- (Todo: persist change to backend)

### Attendance Tracking
- Date picker selects attendance day
- Table rows show student | course | mentor | time in/out | status
- Status quick-toggle: Present (green) | Absent (red) | Late (amber)
- Attendance marked → students auto-update in dashboards

### Commission Management
- Mentor cards show: name | courses | payout status | commission %
- Edit commission: inline form or modal dialog
- Payout calculation: sum of completed classes × commission rate × course price

### Course Levels & Topics
- Each course has 1+ levels (ordered)
- Each level has 1+ topics
- Topics have assets: videos, PDFs, audio, etc.
- Expandable level rows show topics on click
- Add topic button in expanded section

### Real-time Activity Feed
- Dashboard shows recent actions: enrollments, payments, attendance marks, pauses
- Activity items: actor avatar | action text | timestamp
- Automatically updates (todo: WebSocket or polling)

---

## 8. Data Flow

### Enrollment Flow
```
Admin clicks "Enroll Student" → Modal opens
Admin selects student + course + status
Submission → (api call todo) → Student added to course
Course detail refreshes to show new student
Dashboard activity updated
```

### Payment Flow
```
Student enrolls → Invoice created
Invoice appears in Finance section
Invoice sent to student (email todo)
Student pays → Status changes to "Paid"
Payment reflected in Dashboard revenue KPI
Mentor commission calculated
Payout queued for next cycle
```

### Mentor Payout Flow
```
Mentor completes classes → Commission accrued
Dashboard KPI: "Pending Payouts" shows total
Admin reviews pending payouts page
Initiates payment → Status: "Processing"
Payment confirmed → Status: "Completed"
Mentor notified (todo: email)
Payout removed from pending
```

### Class Session Flow
```
Admin schedules session (date, time, student, mentor, course)
Session appears in Class Sessions page + Dashboard
Mentor receives notification (todo: email/SMS)
Session time arrives → Status: "In Progress"
Mentor marks attendance → recorded
Session ends → Status: "Completed"
Attendance reflects in student dashboard
Course progress updates
```

### Admin to Student Data Flow
```
Admin enrolls student in course
├─ Student sees course in "My Courses"
├─ Student joins first class session
├─ Student views course levels/topics
├─ Student attends classes (marked by mentor)
└─ Student sees attendance % + progress

Admin creates invoice
├─ Student receives invoice notification
└─ Student payment updates in admin view

Admin updates student status (pause/cancel)
└─ Student sees status change immediately
```

---

## 9. Important Notes for Future Work

### Client-Side Only (Incomplete)
- **NO backend integration**: All data is hardcoded (see `courses`, `ALL_STUDENTS`, `ALL_MENTORS` in JS)
- **NO persistence**: Changes to modals don't save (modals close but don't actually create/update)
- **NO real-time updates**: Activity feed doesn't auto-refresh
- **NO email notifications**: Confirmations, invoices, payouts not sent
- **NO payment processing**: Invoice payment not integrated with payment gateway

### Architecture Gaps
- **No authentication**: Anyone can access admin portal
- **No role-based access**: All admins see all data
- **No audit logs**: No tracking of who did what when
- **No API layer**: Direct DOM manipulation, no state management (Redux, Vuex, etc.)
- **No validation**: Form inputs not validated server-side

### Known Limitations
- **Color scheme**: Two slightly different color palettes across pages (some pages use `#1a0533` sidebar, others `#16012e` — inconsistent)
- **Typography**: Font size inconsistencies between pages (topbar title ranges 15-16px)
- **Sidebar inconsistency**: Some pages reload sidebar styles, others expect shell's sidebar (index.html sidebar not rendered inside pages)
- **Modal sizing**: Fixed 500px wide, doesn't responsive on smaller screens
- **Scrollbar styling**: Custom scrollbar hidden on most elements, visible on content areas

### TODOs (High Priority)
1. **Backend API**: Implement REST endpoints (POST/PUT/DELETE for courses, students, mentors, sessions, attendance, invoices, payouts)
2. **State Management**: Centralize data (localStorage, sessionStorage, or state management library)
3. **Form Validation**: Client + server validation for all inputs
4. **Authentication**: Login page, JWT tokens, role-based access control
5. **Email Service**: Send invoices, notifications, payouts to mentors
6. **Payment Gateway**: Integrate Razorpay/Stripe for invoice payments
7. **Responsive Design**: Mobile-friendly layouts, touch-friendly modals
8. **Accessibility**: ARIA labels, keyboard navigation, color contrast
9. **Testing**: Unit tests for filters, modals; E2E tests for workflows
10. **Error Handling**: Toast notifications for API failures, retry logic

---

## 10. Development Workflow

### Adding a New Course
1. Open `courses_page.html`
2. Add object to `courses` array (see Data Structures)
3. Fill in: id, name, color, status, description, pricing, model, students, levels, mentors
4. `renderGrid()` auto-renders course card
5. Click card → detail view populated from data

### Adding a Student
1. Open `student_management.html` (or Modal in courses page)
2. Click "New Student" or "Enroll Student"
3. Modal: fill name, email, phone, courses, status
4. Submit → (TODO: POST to backend)
5. Student added to `ALL_STUDENTS` array
6. Appears in student list + course enrollments

### Creating a New Page
1. Create `newpage.html` in root (same level as `dashboard.html`)
2. Copy layout structure from existing page (sidebar + topbar + content)
3. Add data objects + filter/render functions
4. Add nav item to `index.html` sidebar: `<div class="nav-item" data-page="newpage.html">`
5. Navigation auto-works via iframe click handler

### Modifying Navigation
1. Open `index.html`
2. Add/edit nav items in sidebar (`.nav-scroll` section)
3. Add `data-page="filename.html"` attribute
4. Add icon SVG + label text
5. Navbar click handler automatically switches iframe src

### Changing Colors/Styling
1. Edit `:root` CSS variables in page head (or `index.html` for shell)
2. `--accent`, `--green`, `--red`, `--blue`, `--bg`, `--surface` most visible
3. All pages use same variable names (mostly) — one change propagates

### Adding a Filter
1. Add `<select class="fsel">` to list toolbar
2. Add options matching data field values
3. Add `onchange="filterFunction()"` handler
4. In filterFunction: read select value, filter array, re-render
5. Update count tag with filtered length

### Styling a New Component
1. Use existing utility classes (.card, .pill, .btn-primary, .tag, etc.)
2. Follow spacing scale: 8px | 12px | 16px | 20px | 24px
3. Use color variables: `var(--accent)`, `var(--green-bg)`, etc.
4. Border radius: `var(--radius)` for cards, `var(--radius-sm)` for buttons
5. Shadows: `.card` includes `box-shadow:var(--shadow)`

---

## 11. Testing Checklist

### Functionality Testing

**Courses Page**
- [ ] Grid loads 6 courses with correct data
- [ ] Search filters by course name/description
- [ ] Status filter shows Active/Inactive courses only
- [ ] Pricing model filter works for all 3 models
- [ ] Click course card → detail view loads
- [ ] Back button returns to list, search/filters preserved
- [ ] Click level row → topics expand/collapse
- [ ] Add Topic button opens modal
- [ ] Enrolled Students tab shows correct students
- [ ] Change status dropdown works (active → paused, etc.)
- [ ] Pricing History tab shows correct history entries

**Students Page**
- [ ] Student list loads with 247 students
- [ ] Search by name filters list
- [ ] Status filter shows only selected status
- [ ] Click student → detail view loads
- [ ] Enrollment list shows all courses + status
- [ ] Attendance % shows correct color (green >= 80%, amber < 80%)

**Dashboard**
- [ ] KPI cards show correct values (247 students, 12 courses, ₹2.4L revenue)
- [ ] Hero banner displays with actions (links to correct pages)
- [ ] Recent activity feed shows 5 items
- [ ] Today's sessions table shows 5 sessions (3 completed, 2 scheduled)
- [ ] Bottom quick links navigate to correct pages

**Mentors Page**
- [ ] Mentor list shows 6 mentors with avatars
- [ ] Commission rates display (28% custom, 30% default)
- [ ] Courses assigned shows correct list
- [ ] Payout status displays (pending/completed)

**Attendance Page**
- [ ] Date picker loads
- [ ] Attendance records show for selected date
- [ ] Status badges (Present/Absent/Late) color-coded
- [ ] Mark attendance toggles status

### UI/UX Testing

**Sidebar Navigation**
- [ ] All nav items clickable (12 items total)
- [ ] Active state highlights on page load
- [ ] Badges display correct counts
- [ ] Red badges for invoices (3)
- [ ] Hover states work smoothly
- [ ] Sidebar scroll works if content exceeds height

**Modals**
- [ ] Create Course modal opens/closes
- [ ] Form inputs are editable
- [ ] Pricing model dropdown shows 3 options
- [ ] "Number of Classes" field hidden until "Per 10 Classes" selected
- [ ] Submit button disabled until required fields filled (TODO: add validation)
- [ ] Close button (X) works
- [ ] Backdrop click closes modal
- [ ] Escape key closes modal (TODO: add keyboard handler)

**Search & Filter**
- [ ] Search input clears with Backspace
- [ ] Search works with partial text matching
- [ ] Filter dropdowns show all options
- [ ] Multiple filters work together (e.g., Status + Pricing Model)
- [ ] Count tag updates when filters applied
- [ ] "Clear filters" button works (TODO: add)

**Tables**
- [ ] Rows hover highlight properly
- [ ] Columns remain aligned when scrolling horizontally
- [ ] Status pills show correct colors
- [ ] Action buttons (Edit, View, Delete) visible on hover

### Responsive Testing

**Viewport Sizes**
- [ ] 1440px (standard design width) — all elements aligned
- [ ] 1366px (common laptop) — minor overflow OK
- [ ] 1024px (tablet) — modals may exceed width (note for TODO)
- [ ] < 1024px — not designed for this, layout breaks (noted as desktop-only)

**Scrolling**
- [ ] Sidebar scrolls if nav items overflow
- [ ] Content area scrolls (custom scrollbar visible)
- [ ] Modal content scrolls if body exceeds modal height (TODO: test)
- [ ] Topbar does NOT scroll (sticky)

### Data Validation Testing

**Form Fields**
- [ ] Course name input accepts text
- [ ] Price amount input accepts only numbers
- [ ] Date inputs show date picker
- [ ] Dropdowns show selected value
- [ ] Textarea accepts multiline text

**Data Display**
- [ ] Numbers format correctly (₹ prefix, K/L suffixes)
- [ ] Percentages show with % symbol (87%)
- [ ] Dates show in DD MMM YYYY format
- [ ] Phone numbers show with spacing (98765 43210)

### Performance Testing

**Load Time**
- [ ] index.html (shell) loads < 1s
- [ ] Each page loads < 2s on first open
- [ ] Grid rendering: 6 courses < 200ms
- [ ] Table rendering: 250 students < 500ms
- [ ] Modal opens < 100ms (no animation delay)

**Filtering**
- [ ] Search results appear instantly (< 50ms) for 6 courses
- [ ] Filter dropdowns update count immediately
- [ ] No lag when typing in search field

### Browser Compatibility

**Browsers to Test**
- [ ] Chrome (latest)
- [ ] Firefox (latest)
- [ ] Safari (latest, macOS + iOS)
- [ ] Edge (latest)

**CSS Features Used**
- [ ] CSS Grid (course grid layout)
- [ ] Flexbox (sidebar, topbar, cards)
- [ ] CSS Variables (custom properties)
- [ ] Backdrop Filter (modal blur)
- [ ] Box Shadow (shadows)
- [ ] Gradients (buttons, course thumbnails)

**JavaScript Features**
- [ ] ES6 Arrow functions
- [ ] Template literals (backticks)
- [ ] Array methods (map, filter, find, forEach)
- [ ] Event listeners (click, input, change)
- [ ] classList API
- [ ] querySelector/querySelectorAll

### Accessibility Testing (TODO)

- [ ] Keyboard navigation: Tab through all interactive elements
- [ ] Focus states: All buttons have visible focus outline
- [ ] Color contrast: WCAG AA (4.5:1 for text)
- [ ] Screen reader: Test with NVDA/JAWS (structure, labels)
- [ ] ARIA labels: Add to buttons, icons, dynamic content

---

## Development Guidelines

### Naming Conventions
- **Classes**: `.sidebar`, `.topbar`, `.card`, `.btn-primary` (kebab-case)
- **IDs**: `#pageFrame`, `#srch`, `#mo-new-course` (camelCase, prefixed by element type)
- **Variables**: `courses`, `filteredCourses`, `openLevels` (camelCase, plurals for arrays)
- **Functions**: `filterCourses()`, `openDetail()`, `ddToggle()` (camelCase, verb-noun)

### File Organization
- Each page is a self-contained HTML file (no imports)
- Styles embedded in `<style>` tag
- JavaScript embedded in `<script>` tag
- No external dependencies (keep it simple)

### Code Style
- Minified CSS/HTML to reduce file size
- JS uses single-line comments sparingly
- Functions grouped logically: DATA → FILTERS → RENDERING → EVENTS
- Consistent indentation (2-space in JS, minified in CSS)

### Adding New Features
1. **Identify affected files**: Is it one page or multiple?
2. **Follow existing patterns**: Reuse components (modals, filters, tables)
3. **Test thoroughly**: Use checklist above
4. **Document changes**: Update this CLAUDE.md file
5. **Git commit**: "feat: add [feature name]" or "fix: [issue]"

---

## Quick Reference

| Feature | File | Section |
|---------|------|---------|
| Sidebar Navigation | index.html | Lines 79-134 |
| Dashboard KPIs | dashboard.html | Lines 278-315 |
| Course Grid | courses_page.html | Lines 129-130, 641-672 |
| Course Detail | courses_page.html | Lines 674-882 |
| Student List | student_management.html | List view (not fully shown) |
| Mentor Management | mentor_management.html | List view (not fully shown) |
| Modals | All pages | `.mo` class, lines vary |
| Dropdowns | courses_page.html | Lines 482-526 |
| Color Variables | All pages | `:root` CSS block |
| Responsive Grid | courses_page.html | Lines 130 `.course-grid` |

---

**Last Updated**: April 2026  
**Admin Portal Version**: 1.0 (Client-side prototype, no backend)  
**Maintained By**: [Team Name]
