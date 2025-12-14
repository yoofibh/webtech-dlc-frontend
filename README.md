\# Campus Digital Library Catalogue System



Yoofi Egyin Brew-Hammond - 10022300180- BSC. Computer Science

This is my web technologies exam project: a \*\*Digital Library Catalogue System\*\* for a university campus.



The goal is to allow \*\*students\*\* to browse and search books online, and give \*\*librarians/admins\*\* a simple dashboard to manage the catalogue (add, edit, delete books, and track borrowed items).



The project is built with:



\- \*\*Backend\*\*: Node.js, Express, PostgreSQL

\- \*\*Frontend\*\*: Vanilla HTML, CSS and JavaScript (Single Page Application style)

\- \*\*Auth\*\*: JSON Web Tokens (JWT) with role-based access (student / admin)



---



\## 1. Core Features



\### 1. User Accounts \& Authentication



\- Users can \*\*register\*\* and \*\*log in\*\* with email + password.

\- Passwords are hashed using \*\*bcrypt\*\* before saving to the database.

\- After login, the server issues a \*\*JWT token\*\*, which the frontend stores and sends with each protected request.

\- There are two main roles:

&nbsp; - `student` – default role for normal users.

&nbsp; - `admin` – has access to book management and admin actions.



\## Live Deployment

&nbsp;https://github.com/yoofibh/webtech-dlc-frontend.git



\## Backend API

The frontend connects to:

https://webtech-dlc.onrender.com/api



\## test creds

&nbsp; - Student Login is <ggg@gmail.com> with password 123456

&nbsp; - admin login is <ybh@example.com> with password ybh123!



\### 2. Books Catalogue (Student View)



\- Public catalogue where users can:

&nbsp; - View all books.

&nbsp; - See title, author, category, status, and description.

\- Filters and search:

&nbsp; - Search by \*\*title\*\* or \*\*author\*\*.

&nbsp; - Filter by \*\*category\*\*.

&nbsp; - Filter by \*\*status\*\* (`available` or `borrowed`).



\### 3. Admin Dashboard (Librarian View)



Admins can:



\- Add new books to the system.

\- Edit existing books (title, author, category, description, status).

\- Delete books that are no longer in the catalogue.

\- See an overview table of \*\*all books\*\* in the system.



Admin pages and routes are protected by middleware that checks both:



\- \*\*JWT validity\*\*, and

\- \*\*User role\*\* (`admin` only).



\### 4. Borrowing \& Returning Books



\- Logged-in students can \*\*borrow\*\* a book if its status is `available`.

\- When a book is borrowed:

&nbsp; - A record is created in the `borrow\_records` table.

&nbsp; - A default \*\*7-day due date (week 1)\*\* is set.

&nbsp; - The book status changes from `available` ➝ `borrowed`.

\- Books can be \*\*returned\*\* by:

&nbsp; - The student who borrowed it, or

&nbsp; - Any admin user.

\- On return:

&nbsp; - `returned\_at` is set on the borrow record.

&nbsp; - Book status goes back to `available`.



The student-facing UI also shows a \*\*current due date\*\* (if the book is borrowed and still active).



---



\## 2. Tech Stack \& Architecture



\### Backend



\- \*\*Node.js\*\* with \*\*Express.js\*\*

\- \*\*PostgreSQL\*\* database (hosted on Render in my case)

\- Main files:

&nbsp; - `server.js` – bootstraps the app, middleware, routes, and seeds an admin on startup.

&nbsp; - `config/db.js` – PostgreSQL connection pool.

&nbsp; - `config/seedAdmin.js` – ensures there is at least one admin account.

&nbsp; - `routes/authRoutes.js` – handles registration and login.

&nbsp; - `routes/bookRoutes.js` – handles books CRUD and borrow/return logic.

&nbsp; - `middleware/auth.js` – verifies JWT tokens and enforces `admin` role where needed.



\### Frontend



\- \*\*Plain HTML, CSS and JS\*\* (no frontend framework).

\- Single-page style behavior:

&nbsp; - Views:

&nbsp;   - Login

&nbsp;   - Register

&nbsp;   - Books (student view)

&nbsp;   - Admin (manage books)

&nbsp; - `app.js` switches views, calls the backend API, and manages UI state.



\### Database Design



The main tables are:



1\. \*\*users\*\*

&nbsp;  - `id` (PK)

&nbsp;  - `name`

&nbsp;  - `email` (unique)

&nbsp;  - `password\_hash`

&nbsp;  - `role` (`student` or `admin`)

&nbsp;  - `created\_at`



2\. \*\*books\*\*

&nbsp;  - `id` (PK)

&nbsp;  - `title`

&nbsp;  - `author`

&nbsp;  - `isbn`

&nbsp;  - `category`

&nbsp;  - `description`

&nbsp;  - `status` (`available` or `borrowed`)

&nbsp;  - `created\_at`



3\. \*\*borrow\_records\*\*

&nbsp;  - `id` (PK)

&nbsp;  - `user\_id` (FK → users.id)

&nbsp;  - `book\_id` (FK → books.id)

&nbsp;  - `borrowed\_at`

&nbsp;  - `due\_date`

&nbsp;  - `returned\_at` (nullable, set when book is returned)



---



\## Setup \& Running Locally



&nbsp;1. Clone the Repository



git clone <https://github.com/yoofibh/webtech-dlc.git>

cd webtech-dlc

"# webtech-dlc"



&nbsp;1. Install Dependencies



npm install



1\. Set Up Environment Variables



PORT=5000

DATABASE\_URL=<your\_postgres\_connection\_string>

JWT\_SECRET=<your\_secret\_key>



1\. Start the Server



npm run dev



1\. Open your browser and navigate to <http://localhost:5000>



