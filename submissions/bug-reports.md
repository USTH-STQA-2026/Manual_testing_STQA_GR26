# Bug Reports — Báo cáo lỗi

> **Hướng dẫn**: Tạo 1 mục bug cho mỗi TC có kết quả **Fail**.
> Xem [examples/sample-bug-report.md](../examples/sample-bug-report.md) để hiểu cách viết bug report tốt.
> Mỗi bug cần: tiêu đề mô tả hành vi lỗi, bước tái hiện, expected vs actual, severity + giải thích.

| Thông tin | |
|---|---|
| **Nhóm** | `STQA_Group_26` |
| **Ngày báo cáo** | `23/05/2026` |

**Environment:**
- Browser: Edge, Chrome
- Operating system: Windows
- Interface language: Vietnamese

---

## BUG-01: System accepts an invalid email format in the login

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-01 |
| **Related TC** | TC-04 |
| **Related REQ** | REQ-01 |
| **Severity** | Medium |
| **Reported by** | Nguyễn Tiến Dũng, Nguyễn Huy Quang |
| **Date found** | `17/05/2026` |
| **Status** | Open |

**Preconditions:**
- A member account has already been created in the system with an invalid email format: abc@email
- The user is on the login screen

**Steps to reproduce:**
1. Open the system login page.
2. Enter email: abc@email
3. Enter the correct password for the account.
4. Click the Login button.

**Expected result:**
The system must reject the invalid email format according to the rule: `email@domain.ext`  
Display a error notification and not allow to login.

**Actual result:**
The system accepts the email `abc@email` and allows successful login to the system.

**Impact:**
The website doesn't validate email formats during login, which messes with data accuracy, breaks downstream features, and violates the SRS.

**Evidence:**
1. The account with an invalid email format is created by the librarian and can be login successfully.
![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/librarian_creating_account.PNG)

**Suggested fix:**
Add email format validation in:
- account creation function,
- login function.

Only accept emails in the correct format: `email@domain.ext`

---
---

## BUG-02: The category filter is case-sensitive, returning zero results for lowercase inputs.

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-02 |
| **Related TC** | `TC-11` |
| **Related REQ** | `REQ-03` |
| **Severity** | `Medium` |
| **Reported by** | `Nguyễn Phan Hồng Anh` |
| **Date found** | `17/05/2026` |
| **Status** | `Open` |

**Preconditions:**
Successfully logged in to the system and currently in the "Books" tab.

**Steps to reproduce:**
1. Click on the input bar: "Filter by category (e.g. Technology, Economy...)"
2. Enter the all-lowercase string: `"công nghệ"`.
3. Press Enter to apply the filter.
4. Observe the list of books displayed on the UI.

**Expected result:**
The system must process the input in a case-insensitive manner, recognize the category, and display the list of 8 books in the "Technology" group (Book codes BOOK001, BOOK002, BOOK003, BOOK005, BOOK008, BOOK009, BOOK010, BOOK011).

**Actual result:**
The system processes the filter in a case-sensitive manner. The entire book list disappears from the screen, and the interface shows the error message: `'No books found'`. The filter only works when the user types the exact uppercase form `"Công nghệ"`.

**Impact:**
This ruins UX. Users typing in lowercase will assume the system is empty or broken, even though the books exist.

**Evidence:**


<img width="1937" height="1159" alt="image" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/lowercase_filter_error.PNG" />


**Suggested fix:**
Developers need to normalize both the user input string and the book category attribute in the database to the same format (for example, using `.toLowerCase()` or `.toUpperCase()`) before comparing strings.

## BUG-03: The system reports 'Expired member' instead of 'Suspended member' when borrowing books with a member account in the "Suspended" status

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-03 |
| **Related TC** | `TC-12` |
| **Related REQ** | `REQ-04` |
| **Severity** | `Medium` |
| **Reported by** | `Nguyễn Tiến Dũng, Nguyễn Huy Quang` |
| **Date found** | `18/05/2026` |
| **Status** | `Open` |

**Preconditions:**
`Suspended member, book in "Available" status`

**Steps to reproduce:**
1. `Log in to the system with the suspended account`
2. `Borrow any available book`
3. `Check the system response`

**Expected result:**
- The system reports `'Suspended member'`

**Actual result:**
- The system reports: `'Expired member'`

**Impact:**
- Causes incorrect information flow, affecting the librarian’s management process.  
- Affects user experience (UX).

**Evidence:**
<img width="2008" height="1160" alt="REQ-04_TC-04_01" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/not_expired.PNG" />

**Suggested fix:**
Review the branching logic structure and separate the condition clauses for member status checks.

---

## BUG-04: A member can borrow up to 4 books instead of the limit 3 books in the system

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-04 |
| **Related TC** | `TC-16` |
| **Related REQ** | `REQ-04` |
| **Severity** | `High` |
| **Reported by** | `Nguyễn Tiến Dũng, Nguyễn Huy Quang` |
| **Date found** | `18/05/2026` |
| **Status** | `Open` |

**Preconditions:**
`Active member, books in "Available" status`

**Steps to reproduce:**
1. `Log in to the system with an active account`
2. `Borrow 1, 2, 3, 4 books in "Available" status`
3. `Check the system response`

**Expected result:**
`When borrowing the 4th book, the system must report that the 3-book limit has been reached and reject additional borrowing`

**Actual result:**
`The system accepts the 4th book and only rejects and reports an error when borrowing the 5th book.`

**Impact:**
- Violates the business requirement.

**Evidence:**
- Borrowed 4 books:
<img width="2008" height="1160" alt="REQ-04_TC-08_07" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/book_limit1.PNG" />

<img width="2008" height="1160" alt="REQ-04_TC-08_10" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/book_limit2.PNG" />

- And the system reports an error when borrowing the 5th book:

<img width="2008" height="1160" alt="REQ-04_TC-08_11" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/book_limit_notification.PNG" />

**Suggested fix:**
The 3-book limit is broken; users can currently borrow 4 books before getting an error. Please fix the logic condition from <= 3 to < 3.

---

## BUG-05; The system does not display the **overdue warning** when returning an overdue book

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-05 |
| **Related TC** | `TC-19` |
| **Related REQ** | `REQ-05` |
| **Severity** | `Medium` |
| **Reported by** | `Nguyễn Phan Hồng Anh` |
| **Date found** | `18/05/2026` |
| **Status** | `Open` |


**Preconditions:**
`Member has a borrowing slip initially in "Borrowed" status and overdue; overdue book is in "Borrowed" status`

**Steps to reproduce:**
1. `Go to the "Borrow/Return" section`
2. `Click the "Return book" button on the borrowing slip of the overdue book that is still in the "Borrowed" status`
3. `Check the system notification`

**Expected result:**
`The system displays an overdue warning`

**Actual result:**
`The system only displays the return successful message`

**Impact:**
- Violates business requirement BO-02.

**Evidence**

<img width="1912" height="815" alt="REQ-05_baoquahan" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/overdue_date_book.PNG" />

**Suggested fix:**
Check whether the overdue warning has been implemented and whether the display condition for the warning is correct.

---

## BUG-06: The system accepts invalid-format email when adding a member

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-06 |
| **Related TC** | `TC-32` |
| **Related REQ** | `REQ-07` |
| **Severity** | `Medium` |
| **Reported by** | `Nguyễn Phan Hồng Anh` |
| **Date found** | `19/05/2026` |
| **Status** | `Open` |

**Preconditions:**
`Log in as librarian, access the add-member function`

**Steps to reproduce:**
1. `Access the book borrowing system https://stqa.rbc.vn/ and log in as the librarian`
2. `Click the "Add member" icon in the top-right corner`
3. `Enter full name, phone number, and an email missing "."`

**Expected result:**
`The system reports an error because the email syntax is invalid`

**Actual result:**
`Member is added successfully`

**Impact:**
`The system stores incorrect data, which may cause errors when sending borrow/return notification emails later.`

**Evidence:**

![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/invalid_gmail.PNG)

**Suggested fix:**
`Add/check the validation function to block and report the error immediately in the Email field.`

---

## BUG-07: The system not allow to adding a member although with valid information

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-07 |
| **Related TC** | TC-33 |
| **Related REQ** | REQ-07 |
| **Severity** | High |
| **Reported by** | Nguyễn Phan Hồng Anh |
| **Date found** | `19/05/2026` |
| **Status** | Open |

**Preconditions**
- Logged in with the librarian account.

**Steps to reproduce:**
1. Access the book borrowing system https://stqa.rbc.vn/ and log in as the librarian
2. Click the "Add member" icon in the top-right corner
3. Enter complete information with correct syntax

**Expected result:**
The system reports that the member has been added successfully

**Actual result:**
The system reports invalid email

**Impact:**
The add-member function completely loses its core workflow

**Evidence:**
![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/valid_information.PNG)

**Suggested fix:**
Quickly review the API logic in the "Add member" module so the system accepts valid information and saves the member successfully

---

## BUG-08: One member can view another member's private borrow records

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-08 |
| **Related TC** | TC-36 |
| **Related REQ** | REQ-08 |
| **Severity** | High |
| **Discovered by** | Nguyễn Tiến Dũng, Nguyễn Huy Quang, Nguyễn Phan Hồng Anh |
| **Date discovered** | 20/05/2026 |
| **Status** | Open |

**Preconditions:**
- System is at initial seed data state
- Logged in as Member MEM002 (`ba.nguyen@email.com` / `password123`)

**Steps to Reproduce:**
1. Log in as Member MEM002 (`ba.nguyen@email.com` / `password123`)
2. Navigate to the **"Mượn / Trả"** tab
3. Locate the **"Tra cứu phiếu mượn"** (borrow record lookup) search field
4. Enter `MEM006` (the Member ID belonging to `biet.hoang@email.com`) into the search field
5. Observe the records returned

**Expected Result:**
The system returns **no records** or displays an access-denied / unauthorized message. MEM002 must not be able to view BR003 (Quản trị nhân sự hiện đại — belonging to MEM006). *(SRS REQ-08: "Member can only view their own borrow records. NOT allowed to view records of other members.")*

**Actual Result:**
BR003 (**Quản trị nhân sự hiện đại**, borrowed by `biet.hoang` / MEM006, due 15/10/2024, status "Đang mượn") is **fully displayed** to MEM002 — including book title, borrow date, due date, and status.

**Impact:**
Serious privacy and access control violation. Any Member can freely look up and read another member's borrowing history simply by knowing (or guessing) their Member ID. In a real-world deployment, this would constitute a data protection breach.

**Evidence:**
![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/show_other_ticket.PNG)

**Suggested Fix:**
When a Member submits a lookup query, the backend/controller must validate that the searched Member ID matches the currently logged-in user's ID. If it does not match, the system must return an empty result set or an access-denied message. This filter should be enforced server-side (or in the state management layer), not only on the UI.

--


## BUG-09: Member can return other members' books

| Attribute | Details |
|-----------|---------|
| **Bug ID** | BUG-09 |
| **Related TC** | TC-21 |
| **Related REQ** | REQ-05 |
| **Severity** | High |
| **Discovered by** | Nguyễn Tiến Dũng, Nguyễn Huy Quang, Nguyễn Phan Hồng Anh |
| **Date discovered** | 27/05/2026 |
| **Status** | Open |

**Preconditions:**
- The book is in "Borrowed" status
- Logged in as Member MEM002 (`ba.nguyen@email.com` / `password123`)
- Member with the initial borrowing slip in the "Borrowed" status (MEM006)

**Steps to Reproduce:**
1. Log in as member MEM002
2. Navigate to the "Mượn/Trả" tab
3. Click on the "Tra cứu phiếu mượn" and enter "MEM006" in the search bar
4. Click on the "Return book" button on the slip

**Expected Result:**
The system should not allow the member (MEM002) to return the other member' (MEM006) book

**Actual Result:**
The system allows the member (MEM002) to return the other member' (MEM006) book

**Impact:**
Access control violation. Any member can freely return another member' books simply by knowing (or guessing) their Member ID. This might mess with the management of books and create inconveniences for other members.

**Evidence:**
![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/back_other_book.PNG)

**Suggested Fix:**
When a Member submits a lookup query, the backend/controller must validate that the searched Member ID matches the currently logged-in user's ID. If it still shows the other members' borrowing slips, the "Return book" button must not appear or it must not be able to return correctly and display an access-denied message.
