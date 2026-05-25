# Test Summary — Overall Test Report

> **Instruction**: This is a **Quality Assurance** activity — you evaluate the overall quality of the software, not only list bugs.

---

## 1. Group Information

| Item | Information |
|---|---|
| **Group** | STQA_GROUP_05 |
| **Class** | STQA |
| **Report Date** | 23/05/2026 |
| **Tested System** | https://stqa.rbc.vn — v1.0 |

---

## 2. Result Overview

| Metric | Value |
|---|---|
| Total Test Cases | 33 |
| Pass | 21 |
| Fail | 12 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass rate** | 63.64% |
| **Number of bugs detected** | 11 |

### Result by Feature Group

| Feature Group | Total TC | Pass | Fail | Bug | Evaluation |
|---|---|---|---|---|---|
| REQ-01 Login | 7 | 5 | 2 | 2 | Login function works but some validation messages are incorrect. |
| REQ-02 View Book list | 2 | 2 | 0 | 0 | This feature works correctly with no bug found. |
| REQ-03 Search & Filter book | 5 | 5 | 0 | 0 | This feature works correctly with no bug found. |
| REQ-04 Borrow book | 6 | 5 | 1 | 1 | Borrowing function mostly works but one validation message issue was found. |
| REQ-05 & REQ-06 Return & Overdue handling | 4 | 2 | 2 | 2 | Return and overdue handling need improvement due to warning and permission issues. |
| REQ-07 Member management | 6 | 1 | 5 | 5 | This feature has many validation problems and needs major improvement. |
| REQ-08 Borrow record lookup | 3 | 1 | 2 | 1 | This feature has a serious access control issue. |
| **Total** | **33** | **21** | **12** | **11** | **The system has basic functionality, but several validation and access control issues must be fixed.** |


### Bug Distribution by Severity

| Severity | Quantity | Bug IDs |
|---|---|---|
| High | 2 | BUG-05, BUG-11 |
| Medium | 7 | BUG-03, BUG-04, BUG-06, BUG-07, BUG-08, BUG-09, BUG-10 |
| Low | 2 | BUG-01, BUG-02 |

---

## 3. Test Design Techniques Used

| Technique | Applied to which REQ? | Number of TCs used | Explanation of how it was applied |
|----------|------------------------|--------------------|-----------------------------------|
| EP | All 8 REQs | 28 | Applied to **discrete partitions** such as `existing/non-existing` email, `correct/incorrect` password, `empty/non-empty` fields, `Librarian/Member` roles, `Available/Borrowed/Lost` book status, `Active/Suspended/Expired` member status, search `result/no result`, `valid/invalid` member information, and `own/other` borrow records. |
| BVA | REQ-04, REQ-05 | 2 | Applied to **boundary-related rules**: the maximum borrowing `limit of 3 books`, `return date` compared with `due date`, and `overdue checking` based on whether due date is `before/on/after` the current date. |
| Decision Table | REQ-04, REQ-05 | 9 | Applied where **multiple conditions** determine the system action. For REQ-04, it combines `book status`, `member status`, and `borrow count` to decide whether borrowing is `accepted` or `rejected`. For REQ-05, it combines `book ownership` and `return date` to decide whether returning is `accepted`, `rejected`, or `accepted with an overdue warning`. |

---

## 4. Software Quality Analysis

### 4.1. Strengths
- **REQ-01 Login**: Core functions work although some validation messages are incorrect.
- **REQ-02 View Book list**: Book information is displayed correctly and book status updates instantly.
- **REQ-03 Search & Filter book**: Search and filter functions work as expected.
- **REQ-04 Borrow book**: Borrowing rules mostly work correctly, except for one incorrect message for suspended members.
- **REQ-06 Overdue handling**: System can update overdue records correctly.

### 4.2. Weaknesses
- **REQ-05 Return book**: Return permission and overdue warning handling still have issues.
- **REQ-07 Member management**: Multiple validation problems in member creations.
- **REQ-08 Borrow record lookup**: Serious access control issue exists because members can view, return other members’ borrow records.

**Q2. Which bug is the most critical? Why?**
**BUG-003** (Critical) — The librarian cannot add new members. This is the most severe bug because it **completely disables** a core librarian feature. Every valid email is rejected by the system as "invalid", making it impossible for the library to expand its member list for as long as the bug persists.

## 5. Recommended Bug Fix Priority

> 💡 This is the **Quality Assurance** part: you do not only find bugs, but also **recommend the priority order** for fixing them and evaluate their impact.
> Clearly state the prioritization criteria: based on **severity** (technical seriousness) and/or **priority** (business importance).

| Order | Bug | Severity | Reason for Priority |
|-------|-----|----------|---------------------|
| 1 | BUG-11 | High | this affects privacy and business rule |
| 2 | BUG-05 | High | this affects privacy and business rule |
| 3 | BUG-07 | Medium | this forces system store invalid member data |
| 4 | BUG-06 | Medium | Librarian cannot add valid member |
| 5 | BUG-04 | Medium | this forces system miss Overdue warning displayed |
| 6 | BUG-03 | Medium | this rejects borrow action but incorrect warning message |
| 7 | BUG-08 | Medium | this affects UI, user feedback |
| 8 | BUG-09 | Medium | this affects UI, user feedback |
| 9 | BUG-10 | Medium | this affects UI, user feedback |
| 10 | BUG-01 | Low | this affects UI, user feedback |
| 11 | BUG-02 | Low | this affects UI, user feedback |

---

## 6. Conclusion

- **Pros:** The system provides the basic functions required for library management: login, viewing books, searching/filtering books, borrowing books, and checking overdue records.

- **Cons:** Several bugs still need to be fixed, including high-severity access control issues and multiple validation problems. The system needs stronger input validation and better permission control to protect user privacy.

### **The system is not yet ready for release.**

---

## 7. Lessons Learned (Optional)

The process of:
- How to design test cases systematically based on requirements.
- How to execute manual tests and compare expected results with actual results.
- How to write clear bug reports with evidence, impact, severity, and suggested fixes.

→ **Testing should also check validation, user permissions, privacy, and system behavior consistency.**

---

## 8. AI Usage Declaration (Optional)

> If the group used AI tools such as ChatGPT, Copilot, Gemini, etc., clearly state it below. Honest declaration **does not affect the grade** — this is a professional transparency skill.

| AI Tool | Used for which part | How you reviewed/edited the output |
|---|---|---|
| ChatGPT | Brainstorming IDM | 1. Provide a brief description of Library System<br>2. Use REQ-01 as an example to ask for guidance on how to brainstorm IDM and how to structure a small sample IDM section<br>3. The group wrote the IDM for the remaining REQs by ourselves<br>4. Use ChatGPT to check whether any IDMs were missing or unnecessary, and to help with paraphrasing, grammar, spelling |
| ChatGPT | Reviewing Test cases | 1. Members created TCs manually based on the completed IDM and REQs version<br>2. Use ChatGPT to review wording, paraphrase some test objectives and some expected results, check grammar and spelling<br>3. Group combined members' work, removed duplicated TCs and finalized the TC list manually |
| ChatGPT | Reviewing bug reports | 1. Group wrote the bug reports based on the failed test execution results<br>2. Use ChatGPT to check the clarity of descriptions, paraphrase some parts, improve grammar, and make the wording more consistent. |
