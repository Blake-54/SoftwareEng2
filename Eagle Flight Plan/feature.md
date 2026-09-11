# Eagle Flight Plan — System Features

## Actors (in scope)

- **Students** — complete semester checklists of career-prep tasks and events. Each student has a **major** and **one faculty advisor** assigned **when they come to the school**.
- **College professors (faculty advisors)** — **view each advisee’s progress** and **push tasks**.

Department heads, career-center staff, and employers are **out of scope**. The system does **not** have a department-head role that assigns advisors or oversees all students.

### How a professor gets “their” students

1. When a student **enters the school**, they are given a **major** and **one faculty advisor** (a professor for that major).
2. That **advisor** is the only faculty user who sees that student’s progress and pushes tasks onto their checklist.
3. If a major has several professors, each student still has **one** advisor; professors do not share the whole major roster.
4. Other professors cannot see students they do not advise. The system does not assign or reassign advisors through a department-head workflow.

### Standard college checklist (not advisor-invented)

Every student gets the **same kinds of college-standard items** on their plan. Advisors may still **push extra tasks** (F11); they do not replace this list.

Known standard items include:

- Build a **resume**
- Attend **career fairs**
- Create accounts on job apps such as **Indeed** and **Handshake**

Other similar prep work (for example LinkedIn) can sit on the same standard list.

## Parent user stories

**As a** student, **when** I want to do activities that prepare me to get a job when I graduate,  
**I want** a checklist of tasks and events I can complete each semester I am in school,  
**so that** I am prepared to successfully apply for a job.

**As a** professor (faculty advisor), **when** students are assigned to me as advisees when they enter the school,  
**I want** to see each advisee’s checklist progress and push tasks for them to do,  
**so that** I can coach them and keep them on track toward a job.

---

## Feature list

| ID  | Feature                        | Summary                                                                                                   |
| --- | ------------------------------ | --------------------------------------------------------------------------------------------------------- |
| F1  | Student account and profile    | Students sign in and keep academic/career profile data used to tailor the plan.                           |
| F2  | Semester flight-plan checklist | Each semester shows the college-standard tasks and events, plus any extras the advisor pushed.            |
| F3  | Career-prep tasks              | Standard tasks such as resume and Indeed/Handshake accounts; students mark them complete.                 |
| F4  | Career events                  | Standard events such as career fairs; students mark attendance.                                           |
| F5  | Progress tracking              | Students check items off and see how ready they are to apply for jobs.                                    |
| F6  | Multi-semester plan            | Students see a full-school checklist organized by semester from first year through graduation.            |
| F7  | Reminders and deadlines        | Students get reminders so they complete items before the semester ends.                                   |
| F8  | Job-application readiness      | Students see remaining gaps before they apply (resume, internships, skills, documents).                   |
| F9  | Resources and guidance         | Each checklist item can include how-to resources, templates, and office hours.                            |
| F10 | Advisor roster and progress    | Faculty advisors see their advisees (assigned at enrollment) and open each student’s progress.            |
| F11 | Advisor-assigned tasks         | Faculty advisors push tasks onto a student or group of their advisees.                                    |

---

## User stories by feature

### F1 — Student account and profile

**US-1.1**  
As a student, I want to create an account and sign in, so that my checklist and progress are saved to me.

**US-1.2**  
As a student, I want to record my major, expected graduation term, and current semester, so that my checklist matches where I am in school and I am linked to a faculty advisor for that major.

**US-1.3**  
As a student, I want to update my career interests (roles, industries, internships vs. full-time), so that recommended tasks and events stay relevant.

**US-1.4**  
As a student, I want my faculty advisor to be set when I enter the school (based on my major), so that I have someone who can view my progress and push tasks from day one.

**US-1.5**  
As a student, I want to see who my assigned faculty advisor is, so that I know who is coaching me.

---

### F2 — Semester flight-plan checklist

**US-2.1**  
As a student, I want a checklist of college-standard tasks and events for the current semester (plus any extras my advisor added), so that I know what the school expects me to do this term.

**US-2.2**  
As a student, I want to switch between past, current, and upcoming semesters, so that I can review what I already did and what is coming next.

**US-2.3**  
As a student, I want required items distinguished from optional items, so that I prioritize the activities that matter most for job readiness.

---

### F3 — Career-prep tasks

**US-3.1**  
As a student, I want standard career-prep tasks on my checklist, including building a resume and creating accounts on job apps such as Indeed and Handshake, so that I complete the same core prep as other students.

**US-3.2**  
As a student, I want to mark a task as not started, in progress, or complete, so that I can track work I have already begun.

**US-3.3**  
As a student, I want to attach or note evidence of completion (file, link, or confirmation)—for example a resume file or a profile URL—so that my advisor can see that I finished the task.

**US-3.4**  
As a student, I want Indeed and Handshake (and similar job-app accounts) called out as their own checklist items, so that I do not skip setting up the sites I will use to apply.

---

### F4 — Career events

**US-4.1**  
As a student, I want career fairs (and other college career events) on my semester checklist, so that attending them is a planned task, not something I have to remember on my own.

**US-4.2**  
As a student, I want to mark that I attended a career fair or similar event, so that event participation counts toward my semester plan.

**US-4.3**  
As a student, I want event date, time, location, and signup information on the checklist, so that I can actually show up.

---

### F5 — Progress tracking

**US-5.1**  
As a student, I want to check off completed tasks and events, so that I can see remaining work at a glance.

**US-5.2**  
As a student, I want a semester progress summary (for example, percent complete or items remaining), so that I know whether I am on pace this term.

**US-5.3**  
As a student, I want incomplete required items called out, so that I do not miss activities that would hurt my job-application readiness.

---

### F6 — Multi-semester plan

**US-6.1**  
As a student, I want a year-by-year (or semester-by-semester) plan from first year through graduation, so that early semesters build toward a successful job search.

**US-6.2**  
As a student, I want later-semester items (for example, applications and interviews) to depend on earlier items (for example, resume and internship experience), so that I understand the sequence of preparation.

**US-6.3**  
As a student, I want my plan to adjust if my graduation term or current semester changes, so that the checklist stays accurate if I take a lighter load or stay an extra term.

---

### F7 — Reminders and deadlines

**US-7.1**  
As a student, I want due dates or suggested windows on checklist items, so that I complete them during the right part of the semester.

**US-7.2**  
As a student, I want reminders for upcoming events and unfinished required tasks, so that I do not forget them until it is too late to apply successfully.

**US-7.3**  
As a student, I want a view of this week’s and this month’s action items, so that the full multi-year plan is still manageable week to week.

---

### F8 — Job-application readiness

**US-8.1**  
As a student, I want a readiness view that shows whether I have finished the college-standard basics (resume, Indeed/Handshake accounts, career-fair attendance), so that I know if I am ready to apply.

**US-8.2**  
As a student, I want the system to list remaining gaps before I apply, so that I can close them instead of applying unprepared.

**US-8.3**  
As a graduating student, I want a final-semester checklist focused on applications, interviews, and offers, so that my last terms convert preparation into a job.

---

### F9 — Resources and guidance

**US-9.1**  
As a student, I want each task and event to include short instructions or linked resources (templates, career-center pages, examples), so that I can complete the item without hunting for help.

**US-9.2**  
As a student, I want to know which professor assigned a task and any notes they included, so that I know who to follow up with.

---

### F10 — Advisor roster and progress

**US-10.1**  
As a professor, I want to sign in and see the students assigned to me as advisees when they entered the school, so that I only coach my own advisees.

**US-10.2**  
As a professor, I want a roster view of each advisee’s overall and current-semester progress, so that I can spot who is behind without opening every checklist.

**US-10.3**  
As a professor, I want to open one advisee’s checklist (tasks, events, completion status, and evidence), so that I can see exactly what they have and have not done.

**US-10.4**  
As a professor, I want to filter or sort my roster (for example, by semester, percent complete, or incomplete required items), so that I can focus on students who need help first.

**US-10.5**  
As a student, I want my progress visible only to my assigned faculty advisor, so that other professors cannot open my checklist.

---

### F11 — Advisor-assigned tasks

**US-11.1**  
As a professor, I want to push a task onto a single student’s checklist (title, instructions, due date), so that that student has a clear next action.

**US-11.2**  
As a professor, I want to push the same task to several of my advisees at once (or to all of them), so that I do not assign the same work one student at a time.

**US-11.3**  
As a student, I want professor-assigned tasks to show up on my semester checklist separately from the standard plan items, so that I can tell what my professor added.

**US-11.4**  
As a professor, I want to see whether each assigned task is not started, in progress, or complete, so that I know if students did what I asked.

---

## Story mapping (parent → features)

The student parent story is satisfied when a signed-in student can open **this semester’s checklist** of **college-standard items** (resume, career fairs, Indeed/Handshake, and similar), complete **tasks** and **events**, see **progress** across **all semesters**, get **reminders**, use **resources**, and judge **job-application readiness**. Advisors may add extra tasks (F11) on top of that standard list.

The professor parent story is satisfied when a faculty advisor—assigned when the student **comes to school**—can **see each advisee’s progress** (F10) and **push tasks** (F11).
