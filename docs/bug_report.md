# Lemon Clicker Bug Report

**Name: Akari Oh**

**Tickets solved:** 1, 2, 3, 4, 5

---

## Ticket 1: Stats always say "Stats unavailable"

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**
loadStatsFromServer() in the MainScreenController file

**What was wrong:**
There was a typo in the fetch url for "stats"

### Fix

<!-- What did you change, and why does that change fix the problem? -->
I added an "s" to fix the typo in the fetch url from ("/api/stat") to ("/api/stats").


### How did you find it?

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** "Stats unavailable" text on main screen
2. **Traced back to:** MainScreenView statsText and updateStats() function -> found that stats is null
3. **Traced back to:** loadStatsFromServer() in MainScreenController -> checked network and saw the stats was returning 404
4. **Buggy code:** const response = await fetch("/api/stat"); in loadStatsFromServer()

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
The stats line now shows the best score, the average and the number of saved scores. 

---

## Ticket 2: Best score always shows 0

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**
app.get("/api/stats", (_req, res)... function in index.ts

**What was wrong:**
if (entries[i].score < best) has the opposite operation, so it's taking the lowest score

### Fix

<!-- What did you change, and why does that change fix the problem? -->
Changed < to > to take the highest score instead

### How did you find it?

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

I hypothesized that the calculation for best score is wrong, so I looked at the index.ts file in the server and found that the best score in app.get("/api/stats", (_req, res)... function was calculated to get the lowest score with the opposite comparison operator.


### Confirming the fix

<!-- How did you confirm the problem is gone? -->
The stats line now shows the correct best score as 19.

---

## Ticket 3: The lemon only lands in four spots

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**

**What was wrong:**

### Fix

<!-- What did you change, and why does that change fix the problem? -->

### How did you find it?

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** The lemon only appears in the corners of the screen
2. **Traced back to:** GameScreenView
3. **Traced back to:**
4. **Buggy code:** <!-- the file, function and line where the trace ended -->

<!-- It is okay to use your judgement here in limiting the number of traceback steps. We want to understand your reasoning for how you discovered where the bug was -->

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
After each click the lemon moves to a random position anywhere inside the play area.
