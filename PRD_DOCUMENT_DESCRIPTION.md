# Understanding Product Requirements Documents (PRDs)

**Course:** Mobile Apps in Surgery and Medicine 4.0 (AGH University)

---

## What is a PRD?

A **Product Requirements Document (PRD)** is a comprehensive document that defines **what** a product should do (but not **how** to build it). Think of it as the "blueprint" or "instruction manual" for what you're building.

**Simple analogy:** If you were building a house:
- The PRD is like the client saying: "I need a 3-bedroom house with a kitchen, 2 bathrooms, and a garage"
- The architectural plan (our ARCHITECTURE_GUIDE.md) shows **how** to build it
- The code is the actual construction

---

## Why Do PRDs Exist?

### 1. Clear Communication

**The problem:** Without a PRD, everyone has different ideas:
- The client thinks they're getting Feature X
- The developer builds Feature Y
- The tester expects Feature Z
- Result: Confusion, wasted time, unhappy client

**The solution:** A PRD ensures everyone agrees on what's being built **before** writing any code.

### 2. Requirement Traceability

In regulated industries (healthcare, finance, aviation), you must prove that:
- Every requirement was implemented
- Every feature was tested
- Nothing was added that wasn't approved

The PRD is the source of truth for all requirements.

### 3. Project Scope Management

Without a clear scope, projects suffer from **scope creep**:
- "Can we just add this one small feature?"
- "It would be nice if it also did..."
- "While you're at it, can you..."

The PRD defines what's **in scope** and what's **out of scope**.

### 4. Multiple Teams Coordination

Large projects involve:
- Mobile developers (you!)
- Backend developers
- UI/UX designers
- QA testers
- Product managers
- Regulatory compliance officers

The PRD ensures everyone is building toward the same vision.

---

## Anatomy of a PRD

### 1. Executive Summary
**Purpose:** High-level overview for busy stakeholders

**Contains:**
- What is the product?
- Why are we building it?
- Who is it for?
- What success looks like

**Example:**
> "The MedPharm Pain Assessment App collects daily pain scores from 3000 clinical trial patients. Success means 85% daily compliance and zero data loss."

### 2. Product Scope
**Purpose:** Define boundaries - what's included and what's not

**Contains:**
- ✅ In Scope: Features we **will** build
- ❌ Out of Scope: Features we **won't** build (at least not now)
- 🔮 Future Considerations: Nice-to-haves for later

**Why this matters:**
Saying "no" is as important as saying "yes". Otherwise, the project never finishes!

### 3. User Personas
**Purpose:** Understand who you're building for

**Contains:**
- Who are the users?
- What are their goals?
- What are their pain points?
- What's their technical skill level?

**Example:**
> "Sarah, 65-year-old trial participant, moderate smartphone skills, has arthritis (motor challenges), wants to help medical research but finds technology frustrating."

This helps you design with real people in mind, not abstract "users".

### 4. Functional Requirements
**Purpose:** Detailed description of every feature

**Contains:**
- Feature name
- Description
- Priority (P0 = must have, P1 = should have, P2 = nice to have)
- Acceptance criteria (how to know when it's done)
- User flows

**Example:**
```
Feature: Daily Assessment Reminder
Priority: P0 (Must Have)
Description: User receives a notification at their chosen time to complete daily assessment
Acceptance Criteria:
- Notification delivered at selected time (±2 minutes)
- Notification includes motivational message
- Tapping notification opens assessment screen
- User can snooze for 15, 30, or 60 minutes
```

### 5. Non-Functional Requirements
**Purpose:** Define quality attributes, not features

**Contains:**
- Performance (app loads in <3 seconds)
- Reliability (crashes in <0.1% of sessions)
- Scalability (supports 10,000 users)
- Compatibility (iOS 14+, Android 10+)
- Usability (new users complete first assessment in <10 minutes)
- Accessibility (WCAG 2.1 AAA compliance)

These are just as important as features!

### 6. Technical Specifications
**Purpose:** Bridge between "what" and "how"

**Contains:**
- Technology stack (Flutter, SQLite, etc.)
- API specifications
- Data models
- Security requirements

This section starts getting into implementation, but still focuses on requirements.

### 7. Success Metrics
**Purpose:** Define measurable goals

**Contains:**
- Key Performance Indicators (KPIs)
- Target values
- How to measure them

**Example:**
```
Metric: User Compliance Rate
Target: ≥85% of scheduled assessments completed
Measurement: (Completed assessments / Total scheduled) × 100
```

### 8. Risks & Mitigation
**Purpose:** Anticipate problems before they happen

**Contains:**
- What could go wrong?
- How likely is it?
- How bad would it be?
- What can we do to prevent/minimize it?

**Example:**
```
Risk: Low patient compliance due to survey fatigue
Likelihood: High
Impact: High (compromises trial data)
Mitigation:
- Gamification features
- Keep assessments <5 minutes
- Motivational messaging
- Progress visualization
```

---

## PRD vs. Other Documents

### PRD vs. Technical Architecture Document
- **PRD:** *What* to build (product perspective)
- **Architecture:** *How* to build it (developer perspective)

### PRD vs. User Stories
- **PRD:** Comprehensive, formal, complete specification
- **User Stories:** Small, incremental, team-facing tasks

User stories are often **derived from** the PRD.

### PRD vs. Design Mockups
- **PRD:** Describes functionality and behavior
- **Mockups:** Shows visual appearance

Both are needed - PRD says "user can filter by date", mockup shows where the filter button goes.

---

## Different Types of PRDs

### 1. Comprehensive PRD (like ours: PRD.md)
**Use when:**
- Large, complex projects
- Regulated industries (healthcare, finance)
- Multiple teams
- External clients

**Pros:** Complete, unambiguous, traceable
**Cons:** Time-consuming to write

### 2. Lean PRD
**Use when:**
- Small projects
- Startups (fast iteration)
- Internal tools
- Familiar team

**Contains:** Just the essentials:
- Problem statement
- Solution overview
- Key features (bullet points)
- Success metrics

**Pros:** Fast to write, easier to update
**Cons:** More ambiguity, requires more discussion

### 3. User Story Map
**Use when:**
- Agile development
- Collaborative teams
- Evolving requirements

**Format:** Visual board with user stories
**Pros:** Flexible, collaborative
**Cons:** Less formal, harder to track compliance

---

## How to Write a PRD (Simplified)

If you need to write a PRD for a project, follow these steps:

### Step 1: Understand the Problem (30 minutes)
- What problem are we solving?
- Who has this problem?
- Why does it matter?
- What happens if we don't solve it?

### Step 2: Define Success (15 minutes)
- What does "done" look like?
- How will we measure success?
- What are we optimizing for? (speed? quality? cost?)

### Step 3: List Features (1 hour)
- Brainstorm everything the product could do
- Categorize: Must Have / Should Have / Nice to Have
- Focus on user value, not technical details

### Step 4: Write User Personas (30 minutes)
- Who are the primary users?
- What are their goals and frustrations?
- What's their context? (where, when, how using the product)

### Step 5: Detail Each Feature (2-3 hours)
For each Must Have feature:
- Description (what it does)
- Why it matters (value to user)
- Acceptance criteria (how to know it's done)
- Edge cases (what if X happens?)

### Step 6: Define Constraints (30 minutes)
- Technical constraints (must work on iOS/Android)
- Time constraints (launch by March)
- Budget constraints (fixed budget)
- Regulatory constraints (HIPAA compliance)

### Step 7: Identify Risks (30 minutes)
- What could go wrong?
- What's our plan B?

### Step 8: Review and Refine (1 hour)
- Show to stakeholders
- Get feedback
- Clarify ambiguities
- Update document

**Total time: ~6-8 hours for a moderate PRD**

---

## Common PRD Mistakes

### 1. Too Vague
❌ "The app should be fast"
✅ "The app launches in under 3 seconds on iPhone 12"

### 2. Mixing "What" and "How"
❌ "Use a red button that calls the saveUser() API endpoint"
✅ "User can save their profile, with confirmation message"

(The designer decides button color, the developer decides API name)

### 3. No Prioritization
Saying everything is P0 (critical) means nothing is truly critical. Be honest about what's essential vs. nice-to-have.

### 4. No Acceptance Criteria
Without clear criteria, you'll argue forever about whether something is "done".

❌ "User can login"
✅ "User enters email + password, system validates in <2s, redirects to home on success, shows error message on failure"

### 5. Writing for Yourself
PRDs are for everyone: developers, testers, designers, managers, clients. Use clear language, avoid jargon, explain acronyms.

### 6. No Out-of-Scope Section
If you don't explicitly say what's NOT included, people will assume it is. This leads to scope creep.

### 7. Forgetting Non-Functional Requirements
Features are useless if the app crashes constantly. Define performance, reliability, and usability requirements.

---

## PRDs in Agile Development

**Question:** "Isn't Agile about less documentation? Why write a big PRD?"

**Answer:** Agile doesn't mean **no** documentation, it means **right-sized** documentation.

**Agile approach to PRDs:**

1. **Start with a lean PRD**
   - Just the essentials
   - Enough to get started

2. **Iterate as you learn**
   - Add details when needed
   - Update based on user feedback

3. **Use user stories for sprints**
   - Break PRD features into stories
   - Implement incrementally

4. **Keep PRD as source of truth**
   - User stories come and go
   - PRD shows the big picture

**Hybrid approach:**
- Write comprehensive PRD for complex/regulated projects
- Use lean PRD + user stories for everything else

---

## PRDs in Regulated Industries

In healthcare, finance, and aviation, PRDs are **required** for:

### 1. Validation & Verification (V&V)
- **Validation:** Did we build the **right** thing? (PRD requirement → implemented feature)
- **Verification:** Did we build it **correctly**? (feature → test case → pass/fail)

### 2. Regulatory Audits
Auditors ask: "How do you know this feature meets FDA/HIPAA requirements?"

Answer: "PRD requirement 4.7.1 states encryption requirement, architecture doc shows TLS 1.3 implementation, test report proves it works."

### 3. Traceability Matrix
```
PRD Requirement → Design Spec → Code → Test Case → Test Result
    4.1.1       →   Section 3   → auth_service.dart:42 → TC_001 → PASS
```

This proves every requirement was:
- Designed
- Implemented
- Tested
- Verified

### 4. Change Management
If requirements change mid-project:
- Document why (PRD version history)
- Get approval (signatures)
- Update affected systems
- Re-test

Without a PRD, you can't track changes properly.

---

## Example: Comparing Two PRDs

### Bad PRD Example

```
Product: Healthcare App

Requirements:
- Users can login
- Users can see their data
- App is secure
- App is easy to use
- Works on mobile
- Has a nice design
```

**Problems:**
- Vague ("secure" how? "easy to use" measured how?)
- No context (who are the users? what data?)
- No priorities (all equal importance?)
- No acceptance criteria (how to know when done?)
- No details (login with what? password? biometric?)

### Good PRD Example

```
Product: Diabetes Management App

Target Users: Adults with Type 2 diabetes, ages 40-70, moderate tech skills

Requirement 4.2.1: User Authentication
Priority: P0 (Must Have)

Description:
User authenticates using email/password or biometric (Face ID/fingerprint) to access their personal health data.

Functional Requirements:
- Email + password login
- "Remember me" option (stays logged in for 30 days)
- "Forgot password" flow via email reset
- Biometric login (optional, after initial email/password setup)
- Session expires after 7 days of inactivity
- Maximum 3 failed login attempts → 15-minute lockout

Acceptance Criteria:
- Login completes in <3 seconds on 4G connection
- Error messages clearly explain issue ("Incorrect password" not "Login failed")
- Biometric fallback to password if biometric fails
- Complies with HIPAA authentication requirements
- Works offline (biometric only, requires online for password reset)

Edge Cases:
- What if user forgets password and doesn't have email access? → Contact support button
- What if biometric changed (new fingerprint)? → Re-authenticate with password
- What if locked out? → Clear message with countdown timer

Success Metrics:
- Login success rate >95%
- Average login time <2 seconds
- <1% support tickets related to login issues
```

**Why this is better:**
- Specific and measurable
- Clear context
- Prioritized
- Handles edge cases
- Defines success
- Compliance noted

---

## Tools for Writing PRDs

### Simple Options (Free)
1. **Google Docs** - Easy collaboration, commenting
2. **Markdown files** (like this!) - Version control with Git
3. **Notion** - Nice formatting, templates
4. **Confluence** - Wiki-style, good for big teams

### Specialized Tools (Paid)
1. **Aha!** - Product roadmap + PRD
2. **ProductPlan** - Visual roadmaps
3. **Jira** - Integrates with development
4. **Asana** - Task management + requirements

**For this course:** Markdown files in Git are perfect!

---

## PRD Templates

### Minimal PRD Template

```markdown
# [Product Name] PRD

## Problem Statement
What problem are we solving? Who has it?

## Solution Overview
High-level description of the product

## Users
Who will use this?

## Key Features
- Feature 1: [description]
- Feature 2: [description]
- Feature 3: [description]

## Success Metrics
How will we measure success?

## Constraints
Time, budget, technical limitations

## Out of Scope
What we're NOT building
```

### Comprehensive PRD Template

See PRD.md in this project - that's a full template!

---

## Writing User Stories from PRDs

PRDs describe features comprehensively. User stories break them into implementable chunks.

**PRD Requirement:**
```
4.2.3: Daily Assessment Reminders
User receives notification at configured time to complete daily assessment.
```

**Derived User Stories:**
```
Story 1: Configure Notification Time
As a patient, I want to choose when I receive assessment reminders,
so I can complete assessments at a convenient time.
Acceptance Criteria:
- Time picker shows hours and minutes
- Can select any time between 6 AM - 10 PM
- Setting saves to local storage
- Shows confirmation message

Story 2: Deliver Scheduled Notification
As a patient, I want to receive a notification at my chosen time,
so I remember to complete my daily assessment.
Acceptance Criteria:
- Notification delivered within 2 minutes of scheduled time
- Notification includes message "Time for your daily assessment"
- Tapping notification opens assessment screen
- Works even if app is closed

Story 3: Snooze Notification
As a patient, I want to snooze the notification if I'm busy,
so I can complete the assessment later.
Acceptance Criteria:
- Snooze button in notification
- Options: 15 min, 30 min, 1 hour
- Notification reappears after snooze period
- Can snooze maximum 3 times
```

**Pattern:** One PRD requirement → multiple user stories → multiple tasks

---

## For This Lab Assignment

### What You Should Do:

**Option A: No PRD Required** (if this is your first time)
- Just read the provided PRD.md
- Refer to it when implementing features
- Understand what each requirement means

**Option B: Write a Mini PRD** (if you want extra practice)
- Choose 1-2 features not in the PRD
- Write a mini PRD for them
- Include: description, acceptance criteria, user flow
- Use this to guide your implementation

**Option C: Practice Exercise** (good for portfolio)
- Take one section of PRD.md
- Rewrite it in your own words
- Add something you think is missing
- Explain your reasoning

### What You Should NOT Do:

- Write a comprehensive PRD from scratch (too time-consuming)
- Feel overwhelmed by PRD.md's size (it's a reference!)
- Think you need to read every detail (skim for your current feature)

---

## Real-World PRD Examples

Want to see real PRDs? Search for:
- "Open source PRD examples GitHub"
- "[Company] product specification document"
- "Lean PRD template"

Many tech companies share redacted PRDs as examples.

**Study these to see different styles:**
- Amazon: 6-page narrative PRDs
- Google: Design docs with requirements
- Startups: Lean PRDs (2-3 pages)

---

## Summary: Key Takeaways

1. **PRDs define WHAT, not HOW**
   - Product manager writes what to build
   - Developer decides how to build it

2. **PRDs prevent misunderstandings**
   - Everyone agrees before coding starts
   - Reduces wasted work

3. **PRDs are living documents**
   - Update as you learn
   - Track changes with version history

4. **PRD completeness varies by context**
   - Regulated industries: comprehensive
   - Startups: lean
   - Choose what's right for your project

5. **Good requirements are SMART**
   - **S**pecific (not vague)
   - **M**easurable (can verify)
   - **A**chievable (realistic)
   - **R**elevant (solves user problem)
   - **T**ime-bound (clear deadlines)

6. **PRDs connect business to engineering**
   - Business: "We need to increase user engagement"
   - PRD: "Add gamification with points and badges"
   - Engineering: "Implement points system using SQLite"

---

## Further Reading

### Books
- "Inspired" by Marty Cagan - Product management
- "User Story Mapping" by Jeff Patton - Breaking down requirements
- "The Lean Startup" by Eric Ries - Iterative product development

### Articles
- "How to Write a PRD" - Product School
- "Agile Requirements Documentation" - Atlassian
- "FDA Guidance on Software Requirements" - For medical apps

### Practice
- Take any app you use daily
- Write a PRD for one of its features
- Compare your PRD to how the feature actually works
- What did you miss? What assumptions did you make?

---

## Questions to Discuss

Think about these (or discuss with classmates):

1. Why does healthcare software need more detailed PRDs than social media apps?
2. What happens if a critical requirement is missing from the PRD?
3. How do you handle conflicting requirements (Feature A and Feature B can't both exist)?
4. When should you say "no" to a feature request?
5. How detailed is "too detailed" for a PRD?

---

## Conclusion

PRDs might seem like bureaucracy, but they're actually **crucial for success**, especially in:
- Large projects
- Regulated industries
- Teams with multiple stakeholders
- Projects with compliance requirements

**For this lab:**
- Use the provided PRD.md as a reference
- Look up requirements when implementing features
- Notice how detailed specifications help you know what to build

**For your career:**
- Learn to write clear requirements
- Practice breaking features into user stories
- Understand business needs, not just technical solutions

Good software engineering is 50% writing code, 50% understanding what to build. PRDs help with the second part!

---

**Now go read PRD.md with fresh eyes - it should make a lot more sense! 🎯**
