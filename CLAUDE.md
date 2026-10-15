# Violin Studio Tracker - instructions for Claude

Bruce tells Claude about his teaching day or a schedule change in plain words. Claude records it by
creating ONE small JSON file in his Google Drive folder "Violin Studio Inbox"
(find it with the Drive connector: search_files title = 'Violin Studio Inbox' and mimeType = folder) with the Google Drive connector:
create_file, parentId = that folder, contentMimeType "application/json",
disableConversionToGoogleType true, title like "2026-10-14 Tuesday lessons.json".

His Apps Script (processInbox, every 5 minutes and whenever the website loads) reads the file, saves
it to his Google Sheet "Violin Studio Tracker" (find it by title with search_files), and moves it to "Processed".
Bruce does nothing. Never put student names, family names, addresses, Drive or Sheet IDs, keys, or screenshots in this GitHub repo (it is public) (inbox.json stays empty).

Read the sheet first (read_file_content on the sheet found by title) to get exact student names, rates,
the schedule, and open make-ups. Names may be used freely in Drive files (they are private).

## File format
    {"entries":[{"id":"L-YYYYMMDD-n","date":"YYYY-MM-DD",
      "students":[{"name":"Lily","day":"Monday","time":"4:00 PM","length":30,"rate":25,"billing":"Per Lesson","parent":"","phone":"","email":""}],
      "schedule":[{"name":"Max","day":"Wednesday","time":"6:00 PM","length":30}],
      "lessons":[{"name":"Rosa","status":"Attended","charge":12.5,"makeup":false,"notes":""}],
      "payments":[{"name":"Theo","amount":50,"method":"Venmo","for":"October","notes":""}],
      "charges":[{"name":"Kai","description":"Book 2","amount":12}],
      "notes":[{"name":"Nora","piece":"Minuet 2","workedOn":"","practice":"Slow bows","notes":""}],
      "makeups":[{"name":"June","status":"Scheduled","scheduledDate":"YYYY-MM-DD","scheduledTime":"4:00 PM"}]}]}

- Use a new unique id every time (L-YYYYMMDD-1, -2, ...). Items are tagged [id#l0] etc. in Notes,
  so re-sending the same id never duplicates anything. Leave out sections that did not happen.
- lessons.status: Attended, Canceled by Student, Canceled by Teacher, No-Show, Make-Up Lesson.
  Leave out "charge" for normal rules (attended, no-show, student cancel = student's rate;
  teacher cancel = free and creates a make-up; Monthly students are never charged per lesson).
  A taught make-up (status Make-Up Lesson) closes the student's open make-up automatically.
- schedule: {"name","day","time","length"} moves the student's only weekly slot (or the one on
  "fromDay"); "add":true adds a second slot; "remove":true removes slots (optionally only "day");
  "status":"Inactive"/"Active"/"Paused" and "rate" update the student.
- students: adds a new student (skipped if the name exists) plus their weekly slot.
  Rate defaults to $25 per 30 min ($37.50 per 45, $50 per 60).
- payments.method: Cash, Check, Venmo, PayPal, Zelle, Card, Other.
- mileage: {"miles":8,"purpose":"Music store","date":"","name":""} adds a one-off business trip
  (name optional). Lesson mileage is automatic: students with "Miles Per Lesson" set (set per student on the sheet)
  get a Mileage row whenever a lesson is Attended or Make-Up Lesson. Change it with
  schedule {"name","miles":N}. Rate comes from Settings "Mileage Rate (per mile)" (0.76 from
  July 1, 2026; 0.725 before).

## Check photos
When Bruce sends a photo of a check: read the payer name, amount, date, check number and memo.
Match the student by the memo or by the parent/payer name on the Students tab; if unclear, ask.
Record a payment with method "Check", for = memo, notes = "Check #1234". NEVER record the bank
account number or routing number anywhere. Confirm back: who, amount, date, check number.
ALSO, for every check, add an "updates" item so the family's details land on the student record:
  {"name":"Lily","ifBlank":true,"fields":{"Parent Name":"Jane Q Sample","Address":"123 Example St, Anytown, WI 00000"}}
ifBlank keeps anything Bruce already entered. Use every account holder printed on the check
(e.g. "Jane Q Sample / John R Sample").

## updates (any student field)
{"name","fields":{<any Students column>: value},"ifBlank":true|false}. Columns: Student Name,
Parent Name, Phone, Email, Billing Type, Lesson Rate, Status, Start Date, Notes,
Miles Per Lesson, Address.
- Times are text like "5:00 PM". Dates are YYYY-MM-DD in America/Chicago.

After creating the file, tell Bruce in plain text (no markdown symbols) what was recorded, and that it
shows up within 5 minutes or as soon as he opens the website.
