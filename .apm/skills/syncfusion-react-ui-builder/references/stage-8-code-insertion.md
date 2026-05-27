# Stage 8 — Code Insertion

## Overview

Stage 5 generates all code files. Stage 6 installs all packages. This stage inserts files into the IDE in the correct order. The user accepts or rejects each diff.

---

## Pre-Check

Stop and state the reason if any check fails:

| Check | If Failed |
|-------|-----------|
| `component-mapping.json` fully resolved | Hard stop — missing components |
| All packages installed by Stage 6 | Hard stop — complete Stage 6 first |
| Overall Syncfusion theme CSS confirmed in Stage 6 output | Hard stop — theme missing |
| `App.tsx` path is unambiguous | Ask user to confirm target file |

---

## Insertion Order

One file per response. Do not combine steps.

### Step 1 — Component Files

Insert sections in order: **Header → Sidebar → MainContent → Footer**

```
Creating: src/components/Header/Header.tsx
Creating: src/components/Header/Header.css
```

CSS file immediately follows its `.tsx`. No placeholders. Complete code only.

For simple UIs (single component), insert that component + CSS, then go to Step 2.

---

### Step 2 — App.tsx Update

Show changed lines only — never the full file.

```
Updating: src/App.tsx (or main.tsx)
```

```tsx
// 1. Overall Syncfusion theme import (top of file, once)
import '@syncfusion/ej2-[theme-package]/styles/[theme-name].css';

// 2. Component imports
import { Header } from './components/Header/Header';
import { Sidebar } from './components/Sidebar/Sidebar';
import { MainContent } from './components/MainContent/MainContent';
import { Footer } from './components/Footer/Footer';

// 3. Replace App() return block
function App() {
  return (
    <>
      <Header />
      <Sidebar />
      <MainContent />
      <Footer />
    </>
  );
}
```

---

### Step 3 — Build Verification

**Only run if Stage 7 validation was skipped.**
If Stage 7 ran and passed, skip this step — build is already verified.

```
All files inserted. Please run:

npm run build

Paste errors here if any — otherwise you're done.
```

---

## Rollback

- **VS Code / Cursor**: `Ctrl+Z` / `Cmd+Z`
- **Syncfusion Code Studio**: Undo in diff panel
- Or say *"undo the last file"* for regeneration

---

## Success Criteria

| Scenario | Criteria |
|----------|----------|
| Stage 7 ran | Files accepted + theme import in `App.tsx` + layout accepted |
| Stage 7 skipped | Above + `npm run build` passes |