# Devvit Job Matching App — Implementation Tickets

## Overview

Each ticket is a **2-5 minute task** designed to be completed by a subagent in one turn.

---

## Phase 1: Core Post Types (Estimated: 30 min total)

### TICKET-001: Define Job Post Schema
**Time:** 2 min
**File:** `src/types/schemas.ts`
**Task:** 
```typescript
// Add JobPost interface with fields:
// - title: string
// - role: string
// - location: string
// - skills: string[]
// - budget: string
// - type: enum (full-time, part-time, freelance, contract)
// - description: string
// - contact: string
```
**Acceptance:** TypeScript compiles, types exported.

---

### TICKET-002: Define Seeker Post Schema
**Time:** 2 min
**File:** `src/types/schemas.ts`
**Task:** 
```typescript
// Add SeekerPost interface with fields:
// - name: string
// - skills: string[]
// - experience: string
// - availability: enum (immediate, 2-weeks, 1-month, flexible)
// - location: string
// - portfolio?: string
// - bio: string
```
**Acceptance:** TypeScript compiles, types exported.

---

### TICKET-003: Define Category Constants
**Time:** 1 min
**File:** `src/types/schemas.ts`
**Task:**
```typescript
// Export JOB_CATEGORIES constant array:
// ['remote', 'onsite', 'hybrid', 'freelance', 'full-time', 'part-time', 'contract']
```
**Acceptance:** Constant exported and usable.

---

### TICKET-004: Create Job Post Type Handler
**Time:** 5 min
**File:** `src/server/postTypes/jobPost.ts`
**Task:**
```typescript
// Create Devvit PostType for jobs:
// - Import JobPost from schemas
// - Define form fields matching schema
// - Export JobPostType
```
**Acceptance:** File created, PostType registered in main.ts.

---

### TICKET-005: Create Seeker Post Type Handler
**Time:** 5 min
**File:** `src/server/postTypes/seekerPost.ts`
**Task:**
```typescript
// Create Devvit PostType for seekers:
// - Import SeekerPost from schemas
// - Define form fields matching schema
// - Export SeekerPostType
```
**Acceptance:** File created, PostType registered in main.ts.

---

### TICKET-006: Register Post Types in Main
**Time:** 2 min
**File:** `src/server/index.ts` or `src/server/main.ts`
**Task:**
```typescript
// Import and register both PostTypes:
// Devvit.addPostType(JobPostType)
// Devvit.addPostType(SeekerPostType)
```
**Acceptance:** App starts without errors on `npm run dev`.

---

## Phase 2: Matching Logic (Estimated: 20 min total)

### TICKET-007: Create Redis Helper Module
**Time:** 3 min
**File:** `src/server/redis/client.ts`
**Task:**
```typescript
// Create helper functions:
// - indexPost(postId, type, data)
// - findMatches(type, skills)
// - deletePost(postId)
```
**Acceptance:** Helpers exported, Redis configured.

---

### TICKET-008: Implement Job Matching Query
**Time:** 5 min
**File:** `src/server/redis/matching.ts`
**Task:**
```typescript
// findMatchingJobs(skills: string[]): Promise<Match[]>
// - Query Redis for jobs with matching skills
// - Return top 5 matches
// - Score by skill overlap
```
**Acceptance:** Query returns matches from indexed jobs.

---

### TICKET-009: Implement Seeker Matching Query
**Time:** 5 min
**File:** `src/server/redis/matching.ts`
**Task:**
```typescript
// findMatchingSeekers(skills: string[]): Promise<Match[]>
// - Query Redis for seekers with matching skills
// - Return top 5 matches
// - Score by skill overlap
```
**Acceptance:** Query returns matches from indexed seekers.

---

### TICKET-010: Create Match Formatter
**Time:** 2 min
**File:** `src/server/utils/formatters.ts`
**Task:**
```typescript
// formatMatches(matches: Match[], type: 'jobs' | 'seekers'): string
// - Return human-readable match list
// - Include title/name, skills, location
```
**Acceptance:** Formatter outputs readable string.

---

## Phase 3: Menu Actions (Estimated: 15 min total)

### TICKET-011: Create Quick Match Menu Action
**Time:** 5 min
**File:** `src/server/menuActions/quickMatch.ts`
**Task:**
```typescript
// MenuAction for right-click on posts:
// - If job post: find matching seekers
// - If seeker post: find matching jobs
// - Display formatted results
```
**Acceptance:** Action appears in Reddit UI on post right-click.

---

### TICKET-012: Register Menu Action
**Time:** 2 min
**File:** `src/server/index.ts`
**Task:**
```typescript
// Devvit.addMenuAction(QuickMatchAction)
```
**Acceptance:** Action registered, appears in test subreddit.

---

## Phase 4: Triggers (Estimated: 15 min total)

### TICKET-013: Create Post Create Trigger
**Time:** 5 min
**File:** `src/server/triggers/postCreate.ts`
**Task:**
```typescript
// Trigger: on PostCreate for job/seeker types:
// - Detect tags from content (remote, onsite, etc.)
// - Set post flair
// - Index in Redis for matching
```
**Acceptance:** New posts get auto-tagged and indexed.

---

### TICKET-014: Implement Tag Detection
**Time:** 3 min
**File:** `src/server/utils/tagDetection.ts`
**Task:**
```typescript
// detectTags(text: string): string[]
// - Check for keywords: remote, onsite, freelance, full-time, etc.
// - Return matching tags
```
**Acceptance:** Detection returns correct tags for test inputs.

---

### TICKET-015: Register Trigger
**Time:** 2 min
**File:** `src/server/index.ts`
**Task:**
```typescript
// Devvit.addTrigger(PostCreateTrigger)
```
**Acceptance:** Trigger fires on post creation.

---

## Phase 5: UI Components (Estimated: 20 min total)

### TICKET-016: Create Job Post Block
**Time:** 5 min
**File:** `src/client/blocks/JobPostBlock.tsx`
**Task:**
```typescript
// React component for displaying job posts:
// - Render title, role, location, skills, budget
// - Match button
// - Contact button
```
**Acceptance:** Component renders in Devvit playground.

---

### TICKET-017: Create Seeker Post Block
**Time:** 5 min
**File:** `src/client/blocks/SeekerPostBlock.tsx`
**Task:**
```typescript
// React component for displaying seeker profiles:
// - Render name, skills, experience, availability
// - Match button
// - Portfolio link
```
**Acceptance:** Component renders in Devvit playground.

---

### TICKET-018: Create Match Results Block
**Time:** 5 min
**File:** `src/client/blocks/MatchResultsBlock.tsx`
**Task:**
```typescript
// React component for displaying matches:
// - List of matched jobs/seekers
// - Click to view full post
// - Skill overlap highlight
```
**Acceptance:** Component displays matches from API.

---

### TICKET-019: Wire Blocks to Post Types
**Time:** 3 min
**File:** `src/server/postTypes/jobPost.ts`, `src/server/postTypes/seekerPost.ts`
**Task:**
```typescript
// Add render property to PostType definitions:
// - JobPostType.render = JobPostBlock
// - SeekerPostType.render = SeekerPostBlock
```
**Acceptance:** Posts render with custom UI in Reddit.

---

## Phase 6: Testing (Estimated: 15 min total)

### TICKET-020: Create Test Subreddit Post
**Time:** 2 min
**Task:** 
- Run `devvit playtest <test-subreddit>`
- Create test job post via UI
**Acceptance:** Post appears in subreddit.

---

### TICKET-021: Test Job Matching
**Time:** 3 min
**Task:**
- Create test seeker post
- Right-click job post → Quick Match
- Verify seeker appears in results
**Acceptance:** Matching returns correct results.

---

### TICKET-022: Test Tag Detection
**Time:** 3 min
**Task:**
- Create job post with "remote" in title
- Verify post gets "remote" flair
**Acceptance:** Tags auto-applied correctly.

---

## Phase 7: Deployment (Estimated: 10 min total)

### TICKET-023: Build for Production
**Time:** 2 min
**Task:**
```bash
npm run build
```
**Acceptance:** Build completes without errors.

---

### TICKET-024: Upload to Reddit
**Time:** 3 min
**Task:**
```bash
devvit upload
```
**Acceptance:** App uploaded to Reddit Developer Platform.

---

### TICKET-025: Install in Target Subreddit
**Time:** 5 min
**Task:**
```bash
devvit install <target-subreddit>
```
**Acceptance:** App live in subreddit, users can create job/seeker posts.

---

## Summary

| Phase | Tickets | Est. Time |
|-------|---------|-----------|
| Core Post Types | 6 | 30 min |
| Matching Logic | 4 | 20 min |
| Menu Actions | 2 | 15 min |
| Triggers | 3 | 15 min |
| UI Components | 4 | 20 min |
| Testing | 3 | 15 min |
| Deployment | 3 | 10 min |
| **Total** | **25** | **~2 hours** |

---

## Execution Order

Each ticket is independent within its phase. Charlie can execute tickets sequentially or spawn parallel subagents for tickets in the same phase.

**Recommended:** Execute Phase 1 first (blocking), then parallelize Phases 2-4.