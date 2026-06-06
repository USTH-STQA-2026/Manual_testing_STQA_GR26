# Test Execution — Kết quả thực thi kiểm thử

> **Hướng dẫn**: Chạy từng TC trên hệ thống https://stqa.rbc.vn, ghi lại kết quả thực tế.
> Kết luận: **Pass** (kết quả đúng), **Fail** (kết quả sai → tạo bug report), **Blocked** (không thực hiện được vì lỗi khác chặn), **Not Run** (chưa chạy).

| Thông tin | |
|---|---|
| **Nhóm** | `STQA_Group_26` |
| **Ngày thực thi** | `16/05/2026` |
| **Trình duyệt** | Chrome/Edge |
| **Hệ điều hành** | Windows |

---

## Kết quả chi tiết

| TC Code | Functional Group | Expected Result (summary) | Actual Result | Conclusion | Evidence | Bug |
|-------|---------------|---------------------------|-----------------|---------|-----------|----| 
| TC-01 | Login | Successfully log in with valid email and password | The system jumped into the home page and displays the username on the top | Pass | ![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc01.PNG) |-|
| TC-02 | Login | Display an error when the email does not exist | The system displays the message “Member not found” | Pass | ![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc02.PNG) |-|
| TC-03 | Login | Display an error when the password is incorrect | The system displays the message “Incorrect password” | Pass | ![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc03.PNG) |-|
| TC-04 | Login | Reject invalid email format and do not allow login | The system accepts the email `abc@email` after created and logs in successfully | Fail | ![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc04.PNG)  | BUG-01 |
| TC-05 | View book list | The system displays the full book list for both Librarian and Member roles | The system successfully displays the complete book list after login | Pass | ![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc05.PNG) | - |
| TC-06 | View book list | Each book displays complete information including title, author, category, publication year, and status | The system correctly displays complete information for every book in the list | Pass | ![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc05.PNG) | - |
| TC-07 | View book list | The system displays the correct status for each book ("Available", "Borrowed", or "Lost") | The system correctly displays the current status of each book | Pass | ![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc05.PNG) | - |
|TC-08|Book Search and Filter (REQ-03)|Accurate search by both book title and author name. If the input string does not exist, the message `'No books found.'` must be displayed.|The filter works well with specific author names and book titles. When entering the junk string "USTH", the screen hides all books and correctly displays `'No books found.'`.| Pass |<img width="2048" height="1189" alt="REQ-03_TC-01_01" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc08_1.PNG" /> <br><img width="2048" height="1189" alt="REQ-03_TC-01_02" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_08_2.PNG" /> <br><img width="2048" height="1189" alt="REQ-03_TC-01_03" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc08_3.PNG" />|-|
|TC-09|Book Search and Filter (REQ-03)|The search system is case-insensitive. Entering mixed-case text should still correctly filter 2 books by author `"Nguyễn Minh Đức"` and 1 book `"Lập trình Flutter cơ bản"`.|The system normalizes strings correctly and displays the corresponding book cards accurately for both mixed-data strings.| Pass |<img width="2048" height="1189" alt="REQ-03_TC-02_01" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc09.PNG" />|-|
|TC-10|Book Search and Filter (REQ-03)|Entering the category `"Công nghệ"` displays exactly 8 books in this category. Entering a non-existent category displays the message 'No books found.'.|The system correctly filters the list of 8 Technology book codes. When entering an invalid category, the list is empty and the message `'No books found.'` is displayed correctly.| Pass | <img width="2048" height="1189" alt="REQ-03_TC-03_01" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc11.PNG" /> <br> <img width="2048" height="1189" alt="REQ-03_TC-03_02" src="https://github.com/user-attachments/assets/8a2f2f1f-ac4f-4295-93c7-3f5d6f673f82" />|-|
|TC-11|Book Search and Filter (REQ-03)|The category filter is case-insensitive. Entering lowercase `"công nghệ"` should still keep all 8 Technology books visible.|The system is case-sensitive. When entering lowercase "công nghệ", the entire list is hidden and an empty interface is returned together with the message `'No books found.'`.| Fail |<img width="2048" height="1189" alt="REQ-03_TC-04_01" src="https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc11.PNG" />| BUG-02 |
|TC-12|Borrow books|Suspended members are denied borrowing books and are informed of the correct reason for rejection|The member is denied but is shown the wrong reason: "Member expired"| Fail |![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc12.PNG)| BUG-03 |
|TC-13|Borrow books|Expired members are denied borrowing books and are informed of the correct reason for rejection|The member is denied and shown the correct reason| Pass |![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc13.PNG)|-|
|TC-14|Borrow books|Active members are allowed to borrow books and are informed of successful borrowing|The member is allowed to borrow and is informed of success| Pass |![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc14.PNG)|-|
|TC-15|Borrow books|Members are denied borrowing books when the book is in the "Borrowed" status|There is no borrow button for books that are "Borrowed"| Pass |![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc15.PNG)|-|
|TC-16|Borrow books|Members are denied borrowing books after borrowing 3 books|A member can borrow up to 4 books| Fail|![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/book_limit1.PNG)<br>![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/book_limit2.PNG)<br>![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/book_limit_notification.PNG)| BUG-04 |
|TC-17|Borrow books|Members borrow books successfully and are given the correct due date|The member borrows successfully and is given a due date of 14 days from the borrowing date| Pass |![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc17.PNG)|-|
|TC-18|Return books|After returning a book, the book returns to the "Available" status|After returning the book, it has returned to the "Available" status| Pass |![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc18_1.PNG)<br>![Alt text](https://github.com/USTH-STQA-2026/stqa-manual-testing-stqa_group_01/blob/f8a198fd435f32a6d3f8792820ab188d6fd51b1b/screenshots/REQ-05_trasachdangmuon.png)<br>![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc18_2.PNG)|-|
|TC-19|Return books|After returning an overdue book, the system displays an **overdue warning**|After returning an overdue book, the system only displays "return successful"| Fail |![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc19.png)| BUG-05 |
|TC-20|Return books|Returned books are in the "Available" status|The book has returned to the "Available" status| Pass |![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc20.png)|-|
|TC-21|Return books|Must not be able to return other members' books|Can return other members' books| Fail |![Alt](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc21.PNG)|BUG-09|
| TC-22 | Overdue book handling (REQ-06) | BR001 and BR003 change to the "Overdue" status after the librarian clicks "Check overdue" | After clicking "Check overdue", BR001 (dueDate: 15/09/2024) and BR003 (dueDate: 15/10/2024) both display the "Overdue" status | Pass | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc22.PNG) | — |
| TC-23 | Overdue book handling (REQ-06) | Returned slips (BR002, BR005) keep the "Returned" status and are not changed to "Overdue" | After clicking "Check overdue", BR002 and BR005 still display "Returned" — their status is unchanged | Pass | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc22.PNG) | — |
| TC-24 | Overdue book handling (REQ-06) | Member accounts do not see the "Check overdue" button | Logged in with `ba.nguyen@email.com`, in the "Borrow / Return" tab — the "Check overdue" button does not appear in the interface | Pass | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc24.PNG) | — |
| TC-25 | Overdue book handling (REQ-06) | Member MEM002 only sees their own overdue slip (BR001), not MEM006's slip (BR003) | After the librarian clicks "Check overdue" and logs out, MEM002 logs in and sees only their own overdue slip | Pass | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc24.PNG) | — |
| TC-26 | Overdue book handling (REQ-06) | A slip with `dueDate` equal to the current date is marked "Overdue" | Slip BR006 (due date matching the simulated system date) is changed to the "Overdue" status | Pass | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc26.png) | — |
| TC-27 | Overdue book handling (REQ-06) | A slip that is not yet due (`dueDate` > current date) is NOT marked "Overdue" | Slip BR006 (due date in the future) remains in the "Borrowed" status after checking | Pass | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc27.PNG) | — |
| TC-28 | Overdue book handling (REQ-06) | Overdue slips are displayed by default as "Borrowed" BEFORE pressing the button | Before clicking the "Check overdue" button, BR001 and BR003 are still displaying the "Borrowed" status | Pass | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc28.PNG) | — |
| TC-29 |Exclusive Add Member feature (for librarian)|"Add member" icon does not appear|The "Add member" icon does not appear on the member account|Pass|![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc29.PNG)|---|
| TC-30 |Add member with full first and last name| The system reports an error due to missing first and last name information | The system responds with "Name cannot be empty"|Pass|![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc30.PNG)|---|
| TC-31 |Check email syntax validation| The system reports an error because the email is missing "@"| The system responds with "Invalid email"|Pass|![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc31.PNG)|---|
| TC-32 |Check email syntax validation| The system reports an error because the email is missing "."| Member is added successfully even though the email is invalid|Fail|![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/invalid_gmail.PNG)|invalid email was still added successfully|
| TC-33 |Basic functionality test of Add member feature| The system adds a member successfully| The system reports invalid email|Fail|![Alt text](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc33.PNG)|Unable to add a member even when the input information contains no errors|
| TC-34 | Borrow Record Lookup (REQ-08) | Librarian can view all borrow records (BR001–BR005) across all members | Logged in as Librarian → "Mượn / Trả" tab → all 5 records (BR001–BR005) are visible with correct member names, book titles, dates, and statuses | Pass | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc34.PNG) | — |
| TC-35 | Borrow Record Lookup (REQ-08) | Member MEM002 sees only their own records (BR001, BR004) in the default view — no other members' records shown | Logged in as MEM002 → "Mượn / Trả" tab → only BR001 ("Đang mượn") and BR004 ("Đã trả") are displayed; BR002, BR003, BR005 are not visible | Pass | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc35.PNG) | — |
| TC-36 | Borrow Record Lookup (REQ-08) | Member MEM002 enters MEM006's ID in the lookup field — should NOT see BR003 | Logged in as MEM002 → "Mượn / Trả" tab → entered MEM006 in the "Tra cứu phiếu mượn" search field → BR003 (Quản trị nhân sự hiện đại, belonging to Hoàng Cá Biệt) is displayed to MEM002 — unauthorized access | **Fail** | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc36.PNG) | BUG-08 |
| TC-37 | Borrow Record Lookup (REQ-08) | All 5 required fields are present in each borrow record: Record ID, Book, Borrow date, Due date, Status | Logged in as Librarian → inspected BR001 → all fields visible: BR001, "Kiểm thử phần mềm nhập môn", 01/09/2024, 15/09/2024, "Đang mượn" (before clicking "Kiểm tra sách quá hạn") | Pass | ![AltText](https://github.com/USTH-STQA-2026/Manual_testing_STQA_GR26/blob/main/screenshots/tx_tc37.png) | — |
| TC-38 | View book list | When a book status changes, the displayed status is updated immediately in the book list | After borrowing and returning books, the displayed status changes correctly between "Available" and "Borrowed" | Pass | | - |

---
## Test Result Summary

| Metric | Value |
|--------|---------|
| Total Test Cases | 38 |
| Pass | 28 |
| Fail | 9 |
| Blocked | 0 |
| Not Run | 0 |
| **Pass Rate** | 73.68% |

### Result by Functional Group

| Functional Group | Total TCs | Pass | Fail | Pass Rate |
|------------------|-----------|------|------|------------|
| Login | 4 | 3 | 1 | 75.00% |
| Borrow / Return Core Actions | 3 | 3 | 0 | 100.00% |
| Search and Filter Books | 4 | 3 | 1 | 75.00% |
| Borrowing Permission Rules | 6 | 4 | 2 | 66.67% |
| Return-book Behavior | 4 | 2 | 2 | 50% |
| Overdue Handling | 7 | 7 | 0 | 100.00% |
| Add Member / Validation | 5 | 3 | 2 | 60.00% |
| Borrow Record Visibility / Lookup | 4 | 3 | 1 | 75.00% |
