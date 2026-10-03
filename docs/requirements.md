# Sheikh Tracker: Requirements

A living document. It covers the whole roadmap in brief and v0 in detail.
Last updated: 2026-09-30.

## 1. Problem

Quran teachers track what each student memorized and reviewed in a paper notebook, or just from memory. That works for a few students. With 10 to 30 students, it gets hard to answer simple questions: what did Omar memorize last week, what should he review today, and what did I tell him to prepare for next time?

## 2. Users

### Persona: the sheikh (v0 user)

- **Who:** Sheikh Mahmoud (made-up), teaches about 15 children after Asr, three days a week.
- **Goal:** know where each student is and what to give them next, without flipping through a notebook.
- **Today:** a paper notebook per class. Pages get lost, and progress is hard to see.
- **Tech:** comfortable with WhatsApp on his phone, not much else.
- **What he needs:** logging a session during class in under a minute, on his phone, in Arabic.

### Later users (not in v0)

Students, parents and academy admins.

## 3. Main workflow (v0)

1. Add a student.
2. Open the student and see today's task (the part assigned last session).
3. Log the session: new memorization, review, rating, note.
4. Assign the part for next session.
5. Look back at the student's past sessions.

## 4. Roadmap

| Release | Tech | Focus |
|---|---|---|
| v0 (Oct 9) | HTML, CSS, JavaScript, localStorage | The core loop for one sheikh on one browser |
| v1 (Oct 23) | React | Edit and delete, attendance, progress per student, today's and this week's sessions, weekly report. First real teachers test it |
| v2 (Nov 20) | Node, Express, MongoDB | Data on a server, same data on phone and laptop, backup |
| v3 (Dec 11) | Login, validation, tests | Sheikh sign-up and login, each sheikh sees only his own students |
| Later | | Student accounts, parent view, reminders, memorization method settings, paid tier for academies, linking with my Quran Tracker app |

## 5. v0 scope (MoSCoW)

**Must**
- Add a student
- Open a student
- Log a session (date, new memorization range, review range, rating, note)
- Assign the next part
- See a student's past sessions, newest first
- Data survives a page refresh

**Should**
- Show today's task when a student is opened
- Edit or delete a session
- Mark attendance (present, absent, late, excused)

**Could**
- Export all data as a JSON file (backup)
- Extra student info (phone, age)

**Won't (this release)**
- Login and accounts (v3)
- Weekly report and progress charts (v1)
- Search and archive (v1)
- Student or parent views (later)
- Syncing between devices (v2)

## 6. v0 user stories and acceptance criteria

### US-1: Add a student
As a sheikh, I want to add a student by name, so that I can start tracking their memorization.

- Given the name box has "Omar", when I tap Add, then Omar appears in the student list and the box is cleared.
- Given the name box is empty or only spaces, when I tap Add, then nothing is added.
- Given two students have the same name, when I add the second one, then both appear as separate students.

### US-2: Open a student
As a sheikh, I want to open a student's page, so that I can see where they are before the session starts.

- Given a student exists, when I tap their name, then I see their name, today's task (if any) and their past sessions.

### US-3: Log a session
As a sheikh, I want to log what a student memorized and reviewed today, so that I can follow their progress over time.

- Given a student is open, when I start a new session, then the date is set to today and I see fields for new memorization (surah, from ayah, to ayah), review (surah, from ayah, to ayah), rating and an optional note.
- Given all required fields are filled, when I tap Save, then the session appears at the top of the student's history.
- Given a required field is empty, when I tap Save, then nothing is saved and a message names the missing field.
- Given "from ayah" is bigger than "to ayah" (for example 20 to 5), when I tap Save, then nothing is saved and a message says the range is wrong.

### US-4: Assign the next part
As a sheikh, I want to set what the student should prepare for next session, so that I don't have to remember it.

- Given I am logging a session, when I fill the "next part" fields and save, then the next part is stored with that session.
- Given a student's last session has a next part, when I open that student, then it shows as today's task.

### US-5: See past sessions
As a sheikh, I want to see a student's past sessions, so that I can check how they are doing.

- Given a student has sessions, when I open them, then all their sessions show newest first, with date, ranges, rating and note.
- Given a student has no sessions yet, when I open them, then I see "No sessions yet".

### US-6: Keep data after refresh
As a sheikh, I want my data to stay after I close or refresh the page, so that I never lose a class record.

- Given I added students and sessions, when I refresh the page, then everything is still there.

## 7. Non-functional requirements (v0)

- **Arabic, right to left:** all labels in Arabic, layout RTL.
- **Mobile first:** works well on a phone screen, with big buttons.
- **Fast:** logging a full session takes under a minute.
- **Simple:** plain Arabic words, no technical terms.
- **No data loss:** every save is written to localStorage right away.
- **Privacy:** made-up student names only until v3 adds login.

## 8. Data sketch

```js
// Student
{
  id: "s_1",
  name: "Omar",
  createdAt: "2026-10-01"
}

// Session
{
  id: "ses_1",
  studentId: "s_1",
  date: "2026-10-01",
  newPart:  { surah: 78, fromAyah: 1, toAyah: 10 },
  review:   { surah: 79, fromAyah: 1, toAyah: 46 },
  rating: "good",            // "excellent" | "good" | "decent" | "bad"
  note: "",
  nextPart: { surah: 78, fromAyah: 11, toAyah: 20 },
  attendance: "present"      // Should: "present" | "absent" | "late" | "excused"
}
```

Surahs are stored by number (1 to 114). The page shows the Arabic name from a surah list.

## 9. Assumptions and open questions

**Assumptions**
- One sheikh, one browser.
- One new-memorization range and one review range per session.
- The four-level rating is enough.

**Questions for a real teacher**
- Do you track by ayahs, pages or juz?
- Do students ever do more than one range in a session?
- Which grades do you actually use?
- Do you record attendance today?
- Would you use this on your phone during class, or after class on a laptop?

## 10. Changelog

- 2026-09-30: first version.