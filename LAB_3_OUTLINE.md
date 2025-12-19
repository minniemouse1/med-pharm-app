# Lab 3 Outline: Clinical Trial Pain Assessment App

**Course:** Mobile Apps in Surgery and Medicine 4.0
**Institution:** AGH University of Science and Technology
**Lab Duration:** 3-4 weeks (suggested)

---

## Overview

In this lab, you'll build a Flutter mobile application for collecting pain assessments in a clinical trial. This is a real-world scenario - pharmaceutical companies actually use apps like this to gather patient data during drug trials.

**The Scenario:**
MedPharm Corporation (fictional) is testing a new pain medication called "Painkiller Forte" in a Phase III clinical trial. They need a mobile app for patients to report their pain levels daily using standardized medical questionnaires.

**Your Role:**
You're the mobile development team building this app. The company has provided you with a Product Requirements Document (PRD.md) and you need to implement the features.

---

## What You'll Learn

By completing this lab, you will:

1. **Real-world architecture patterns**
   - Feature-first project organization
   - Separation of concerns (Models, Services, Providers, UI)
   - How professional apps are structured

2. **State management with Provider**
   - Understanding ChangeNotifier
   - Provider vs Consumer
   - When to use context.read() vs context.watch()

3. **Persistent data storage**
   - SQLite database setup
   - CRUD operations (Create, Read, Update, Delete)
   - Database migrations

4. **Offline-first design**
   - Why apps should work without internet
   - Local-first data storage
   - Background sync strategies

5. **Medical software considerations**
   - Data integrity (zero data loss)
   - Audit trails
   - User accessibility
   - Regulatory compliance basics

---

## Prerequisites

Before starting, you should:

- Have 7+ months of Flutter experience
- Be comfortable with:
  - Dart basics (classes, async/await, streams)
  - Flutter widgets (StatelessWidget, StatefulWidget)
  - Basic navigation
  - Form handling
- Have completed previous labs in this course

If you're rusty on any of these, review them first!

---

## Project Structure

The project is organized in phases with **gradual scaffolding reduction**:

```
Phase 1: Authentication (80% scaffolded)
├── Learn by filling in TODOs
├── Follow the patterns from complete examples
└── Get comfortable with the architecture

Phase 2: Assessment (50% scaffolded)
├── More independence
├── Use Phase 1 as a reference
└── Implement similar patterns

Phase 3: Gamification (30% scaffolded)
├── Greatest independence
├── Design your own solutions
└── Apply everything you've learned
```

This approach helps you build confidence gradually rather than being thrown into the deep end.

---

## Lab Phases

### Phase 1: User Authentication & Enrollment (Week 1)

**Goal:** Get the app working with user enrollment and consent

**What's already done:**
- Database service (complete)
- Project structure and folders
- Model class skeletons
- Service method signatures
- UI screens (layout complete)

**What you need to do:**
1. Implement UserModel methods
   - `toMap()` - Convert model to database format
   - `fromMap()` - Create model from database
   - `copyWith()` - Create modified copies

2. Implement AuthService methods
   - `enrollUser()` - Save user to database
   - `getCurrentUser()` - Fetch current user
   - `updateConsent()` - Update consent status
   - `isEnrolled()` - Check if user enrolled

3. Implement AuthProvider state management
   - Connect service to UI
   - Handle loading states
   - Handle errors
   - Trigger UI rebuilds with notifyListeners()

4. Wire up UI screens
   - Connect buttons to provider methods
   - Display loading indicators
   - Show error messages
   - Navigate on success

**Files to complete:**
- `lib/features/authentication/models/user_model.dart`
- `lib/features/authentication/services/auth_service.dart`
- `lib/features/authentication/providers/auth_provider.dart`
- `lib/features/authentication/screens/enrollment_screen.dart`
- `lib/features/authentication/screens/consent_screen.dart`

**How to approach it:**
1. Read the TODO comments carefully - they contain hints
2. Look at example methods that are already complete
3. Follow the same pattern for incomplete methods
4. Test each method as you complete it
5. Don't move to the next file until the current one works

**Success criteria:**
- User can enter an enrollment code
- App saves user to database
- User can view and accept consent
- Navigation works between screens
- No crashes or errors

---

### Phase 2: Pain Assessments (Week 2)

**Goal:** Implement daily pain assessment questionnaires

**What you'll build:**
1. **Assessment models**
   - AssessmentModel for storing assessment data
   - Conversion to/from database

2. **Assessment service**
   - Save assessments to SQLite
   - Retrieve today's assessment
   - Get assessment history
   - Check if assessment already completed today

3. **Assessment provider**
   - Track current assessment state
   - Handle multi-step questionnaire flow
   - Validate all questions answered
   - Submit completed assessment

4. **Questionnaire UI**
   - NRS screen (0-10 pain scale)
   - VAS screen (visual analog slider)
   - McGill questionnaire
   - Custom questions

**New concepts:**
- Multi-step forms
- Form validation
- Date/time handling
- Complex database queries (filtering by date)

**Success criteria:**
- User can complete all four questionnaires
- Assessment saves to database with timestamp
- Can't complete multiple assessments per day
- Can view assessment history
- Assessment data persists after app restart

---

### Phase 3: Gamification & Progress (Week 3)

**Goal:** Add engagement features to encourage daily assessments

**What you'll build:**
1. **Points system**
   - Award points for completing assessments
   - Bonus points for early completion
   - Store points in database

2. **Level system**
   - Calculate level from total points
   - Display level badge
   - Level-up notifications

3. **Achievement badges**
   - Streak badges (3-day, 7-day, 30-day)
   - Milestone badges (1st, 10th, 50th assessment)
   - Badge gallery UI

4. **Progress visualization**
   - Calendar view of completed assessments
   - Completion percentage
   - Trend charts

**New concepts:**
- Complex calculations from data
- Badge/achievement logic
- Calendar UI
- Data visualization

**Success criteria:**
- Points awarded automatically after assessment
- Level displayed and updates correctly
- Badges unlock at right milestones
- Calendar shows completion history
- Progress stats are accurate

---

### Optional: Phase 4 - Advanced Features (Week 4)

If you finish early or want extra challenge:

**Background sync simulation**
- Mock API service
- Sync queue implementation
- Retry logic for failed syncs
- Sync status indicators

**Notifications**
- Daily reminder notifications
- Scheduled local notifications
- Custom notification sounds

**Enhanced accessibility**
- Screen reader support
- High contrast mode
- Larger text options

**Testing**
- Unit tests for services
- Widget tests for screens
- Integration tests

---

## Getting Started

### Step 1: Project Setup (30 minutes)

1. **Clone/download the project**
   ```bash
   cd MedPharmApp
   ```

2. **Install dependencies**
   ```bash
   flutter pub get
   ```

3. **Verify setup**
   ```bash
   flutter doctor
   flutter devices
   ```

4. **Run the starter app**
   ```bash
   flutter run
   ```

   You should see a basic counter app. This confirms Flutter is working.

### Step 2: Understand the Project (1 hour)

1. **Read the documentation in this order:**
   - README.md (overview)
   - This file (LAB_3_OUTLINE.md)
   - ARCHITECTURE_GUIDE.md (study this carefully!)
   - PRD_DOCUMENT_DESCRIPTION.md (understand what PRDs are)
   - Skim PRD.md (reference, don't read every detail)

2. **Explore the project structure:**
   - Look at lib/features/authentication folder
   - Open user_model.dart - see the TODOs
   - Check out the screens - see the UI layouts
   - Find the database service in lib/core/services

3. **Run the app and explore:**
   - Try to navigate (probably crashes - that's OK!)
   - Identify what's missing
   - Think about how pieces connect

### Step 3: Start Phase 1 (Rest of Week 1)

1. **Start with models:** user_model.dart
   - Simplest starting point
   - Practice toMap/fromMap pattern

2. **Move to services:** auth_service.dart
   - Use the database service
   - Implement CRUD operations

3. **Then providers:** auth_provider.dart
   - Connect service to UI
   - Handle state changes

4. **Finally UI:** screens
   - Wire up buttons
   - Display state

---

## Tips for Success

### General Tips

1. **Read comments carefully** - The TODOs contain hints and guidance
2. **Follow the patterns** - Look at complete examples, then copy the pattern
3. **Test frequently** - Run the app after each major change
4. **One file at a time** - Finish one completely before moving to next
5. **Use print() for debugging** - Print variables to understand what's happening
6. **Ask questions early** - Don't struggle for hours, ask your instructor

### Common Mistakes to Avoid

1. **Skipping the documentation** - Read ARCHITECTURE_GUIDE.md thoroughly
2. **Jumping ahead** - Complete Phase 1 before starting Phase 2
3. **Forgetting notifyListeners()** - Providers won't update UI without it
4. **Not testing** - Test each method before moving on
5. **Copying without understanding** - Understand WHY the pattern works
6. **Ignoring errors** - Read error messages carefully, they tell you what's wrong

### Debugging Strategies

**When you get errors:**

1. **Read the error message** - It usually tells you exactly what's wrong
2. **Check the line number** - Go to that line in the file
3. **Common errors:**
   - "Provider not found" → Check main.dart MultiProvider setup
   - "Table doesn't exist" → Database not initialized properly
   - "Null value" → Check if data exists before accessing it
   - "setState after dispose" → Check widget lifecycle

4. **Use Flutter DevTools:**
   ```bash
   flutter run
   # Then press 'o' to open DevTools in browser
   ```

5. **Check the logs:**
   ```bash
   flutter logs
   ```

### Where to Get Help

1. **Documentation in this project:**
   - ARCHITECTURE_GUIDE.md for patterns
   - PRD.md for feature requirements
   - Code comments for specific hints

2. **Official resources:**
   - [Flutter Documentation](https://docs.flutter.dev)
   - [Provider Package Docs](https://pub.dev/packages/provider)
   - [SQLite Tutorial](https://www.sqlitetutorial.net)

3. **Your instructor:**
   - Office hours
   - Email
   - During lab sessions

4. **Your classmates:**
   - Form study groups
   - Discuss approaches (but write your own code!)

---

## Grading Criteria

Your lab will be assessed on:

### Functionality (40%)
- App runs without crashing
- All required features implemented
- Features work as specified in PRD
- Data persists correctly
- Edge cases handled

### Code Quality (30%)
- Follows the architecture patterns
- Clean, readable code
- Meaningful variable names
- Appropriate comments
- No code duplication
- Proper error handling

### Understanding (20%)
- Can explain architecture decisions
- Understands Provider pattern
- Knows why offline-first matters
- Can discuss trade-offs

### Testing (10%)
- App tested on emulator/device
- Edge cases considered
- No obvious bugs
- Graceful error handling

---

## Deliverables

**What to submit:**

1. **Complete source code**
   - All implemented features
   - No uncommitted changes

2. **Brief report (1-2 pages) including:**
   - What you implemented
   - Challenges you faced
   - How you solved them
   - What you learned
   - Screenshots of working app

3. **Short demo video (5 minutes)**
   - Show app working
   - Demonstrate key features
   - Explain one interesting part of your code

**Submission format:** ZIP file or Git repository link

**Deadline:** [Your instructor will specify]

---

## Timeline Suggestion

### Week 1: Authentication
- Day 1-2: Setup, read documentation, understand architecture
- Day 3-4: Implement models and services
- Day 5: Implement provider
- Day 6-7: Wire up UI, test thoroughly

### Week 2: Assessment
- Day 1-2: Assessment models and services
- Day 3-4: Assessment provider and flow logic
- Day 5-6: Questionnaire UI screens
- Day 7: Testing and bug fixes

### Week 3: Gamification
- Day 1-2: Points and levels system
- Day 3-4: Badges and achievements
- Day 5-6: Progress visualization
- Day 7: Polish and testing

### Week 4: Polish & Report
- Day 1-2: Bug fixes, edge cases
- Day 3-4: Write report, create demo video
- Day 5: Final testing
- Day 6-7: Buffer for any issues

This is just a suggestion - adjust based on your schedule!

---

## Learning Outcomes

After completing this lab, you should be able to:

1. **Structure a professional Flutter app**
   - Organize code in a feature-first architecture
   - Separate concerns properly
   - Make architectural decisions

2. **Implement state management**
   - Use Provider for reactive UI
   - Handle complex application state
   - Debug state-related issues

3. **Work with persistent data**
   - Design database schemas
   - Implement CRUD operations
   - Handle data migrations

4. **Build real-world features**
   - Multi-step forms
   - Offline-first sync
   - Gamification systems
   - Data visualization

5. **Understand medical software**
   - Why data integrity matters
   - Accessibility requirements
   - Regulatory considerations

6. **Professional skills**
   - Reading requirements documents
   - Following coding standards
   - Testing thoroughly
   - Documenting your work

---

## Beyond This Lab

**This lab prepares you for:**

- Mobile app development internships
- Real-world Flutter projects
- Medical/healthcare app development
- Understanding clean architecture
- Technical interviews

**Next steps after completing this lab:**

1. Add the optional Phase 4 features
2. Refactor to full Clean Architecture (use TECHNICAL_ARCHITECTURE.md)
3. Deploy to app stores
4. Add real backend API integration
5. Implement automated testing
6. Contribute to open-source Flutter projects

---

## Questions?

If anything in this outline is unclear:

1. Check the relevant documentation file first
2. Ask in the lab session
3. Email your instructor
4. Discuss with classmates

Remember: Asking questions is a sign of engagement, not weakness. Good developers ask lots of questions!

---

**Ready to start? Head to README.md and follow the Getting Started section. Good luck!**
