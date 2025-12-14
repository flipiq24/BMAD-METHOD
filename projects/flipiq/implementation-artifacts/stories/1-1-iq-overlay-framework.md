# Story 1.1: iQ Overlay UI Framework

**Epic:** Epic 1 - Universal Interface (iQ Overlay)
**Status:** ready-for-dev
**Priority:** P0
**Phase:** 2
**Estimated Effort:** 3 days

---

## Story

**As a** FlipIQ user (AA/AM/Principal)
**I want** an iQ overlay that slides from the right side of my screen
**So that** I can interact with FlipIQ without leaving my current context

---

## Acceptance Criteria

### AC1: Overlay Activation
```gherkin
Given I am logged into FlipIQ
When I click the iQ icon in the toolbar
Then the overlay slides in from the right within 300ms
And the overlay width is 400px on desktop
And the overlay occupies full width on mobile (<768px)
```

### AC2: Overlay Dismissal
```gherkin
Given the overlay is open
When I click outside the overlay OR press ESC key
Then the overlay slides closed within 200ms
And my current page state is preserved
```

### AC3: Context Preservation
```gherkin
Given I am on the Deal Review page with filters applied
When I open and close the iQ overlay
Then my filters remain applied
And my scroll position is preserved
```

### AC4: Responsive Behavior
```gherkin
Given I resize my browser window
When the overlay is open
Then it adjusts responsively:
  - Desktop (>1024px): 400px fixed width
  - Tablet (768-1024px): 350px width
  - Mobile (<768px): Full width with close button
```

### AC5: Keyboard Navigation
```gherkin
Given the overlay is open
When I press Tab
Then focus moves through interactive elements in logical order
And pressing Shift+Tab reverses the order
```

### AC6: Accessibility
```gherkin
Given the overlay is open
When a screen reader reads the page
Then the overlay is announced as a dialog
And the iQ icon has aria-label "Open iQ Assistant"
```

---

## Technical Notes

### Architecture Reference
- See `architecture.md` Section 4.1 - Bot Architecture
- Overlay is the container for AA0 (Universal Interface Bot)

### Implementation Pattern
```
┌─────────────────────────────────────────┐
│           FlipIQ Application            │
│  ┌──────────────────┬────────────────┐  │
│  │   Main Content   │   iQ Overlay   │  │
│  │   Area           │   (400px)      │  │
│  │                  │                │  │
│  │   [Deal Review]  │   [Chat]       │  │
│  │   [Pipeline]     │   [Commands]   │  │
│  │   [PIQ]          │   [History]    │  │
│  │                  │                │  │
│  └──────────────────┴────────────────┘  │
└─────────────────────────────────────────┘
```

### Component Structure
```typescript
// Suggested component hierarchy
<IqOverlayProvider>
  <IqOverlayTrigger />  // Toolbar icon
  <IqOverlayPanel>      // Sliding panel
    <IqHeader />        // Title, close button
    <IqChatArea />      // Conversation history
    <IqInputArea />     // Text/voice input
  </IqOverlayPanel>
</IqOverlayProvider>
```

### State Management
- Use React Context for overlay open/close state
- Persist chat history in local state (not localStorage initially)
- Consider Redux if state becomes complex

### CSS Considerations
```css
/* Slide animation */
.iq-overlay {
  transform: translateX(100%);
  transition: transform 300ms ease-out;
}

.iq-overlay.open {
  transform: translateX(0);
}
```

### Dependencies
- No external dependencies required
- Use existing UI component library (if available)

---

## Tasks / Subtasks

- [ ] **Task 1: Create Overlay Container Component** (AC: 1, 2)
  - [ ] Create IqOverlayProvider context
  - [ ] Implement slide animation CSS
  - [ ] Add open/close state management
  - [ ] Handle ESC key dismissal
  - [ ] Handle click-outside dismissal

- [ ] **Task 2: Create Overlay Trigger** (AC: 1)
  - [ ] Add iQ icon to toolbar
  - [ ] Wire click handler to open overlay
  - [ ] Add aria-label for accessibility

- [ ] **Task 3: Implement Responsive Behavior** (AC: 4)
  - [ ] Add media queries for tablet/mobile
  - [ ] Create full-width mobile layout
  - [ ] Add visible close button for mobile
  - [ ] Test across breakpoints

- [ ] **Task 4: Context Preservation** (AC: 3)
  - [ ] Verify no state reset on open/close
  - [ ] Test with Deal Review filters
  - [ ] Test with Pipeline views
  - [ ] Test scroll position preservation

- [ ] **Task 5: Accessibility Implementation** (AC: 5, 6)
  - [ ] Add role="dialog" to overlay
  - [ ] Implement focus trap when open
  - [ ] Add aria-labelledby for dialog title
  - [ ] Test with screen reader
  - [ ] Verify keyboard navigation

- [ ] **Task 6: Unit Tests**
  - [ ] Test overlay open/close
  - [ ] Test keyboard interactions
  - [ ] Test responsive breakpoints
  - [ ] Test accessibility attributes

---

## Definition of Done

- [ ] All acceptance criteria pass
- [ ] Unit tests written and passing
- [ ] Responsive design verified on Chrome, Safari, Firefox
- [ ] Accessibility audit passes (WCAG 2.1 AA)
- [ ] Code reviewed and approved
- [ ] No console errors in production build

---

## Dev Agent Record

| Field | Value |
|-------|-------|
| Story ID | 1-1 |
| Started | |
| Completed | |
| Blockers | |
| Notes | |

---

*Generated using BMAD Method v6*
