# Lemon Clicker Bug Report

**Name: Akari Oh**

**Tickets solved:** 1, 2, 3, 4, 6

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
randomizeLemonPosition() in GameScreenView

**What was wrong:**
The parenthesis for Math.round() did not include all of the expression when calculating the constants randomX and randomY.

### Fix

<!-- What did you change, and why does that change fix the problem? -->
Moved the closing parenthesis to the end: Math.round(Math.random() * (maxX - minX) + minX);

### How did you find it?

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

I hypothesized that something was wrong with the calculation for randomizing the lemon positions, so I went to randomizeLemonPosition() function in GameScreenView and found that the expression for calculating randomX and randomY had the parenthesis wrong.

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
After each click the lemon moves to a random position anywhere inside the play area.

---

## Ticket 4: Stuck on the Game Over screen

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**
endGame() in GameScreenController file

**What was wrong:**
this.stopTimer; function is missing the parenthesis at the end so it's never called.

### Fix

<!-- What did you change, and why does that change fix the problem? -->
Added the parenthesis for this.stopTimer(); so that the timer is stopped and doesn't keep decrementing and loading the results screen over and over again.

### How did you find it?

#### Backward tracing

<!-- Start from the symptom you saw and work backwards, one step at a time,
     until you reach the buggy line. For each step, note where you looked
     and what you saw there. Add steps as needed. -->

1. **Symptom:** the leaderboard was being fetched infinitely in the network and the gameover sound was playing infinitely
2. **Traced back to:** showResultsFromServer() in ResultsScreenController must have been called multiple times -> only called in main when screen switches
3. **Traced back to:** endGame() in GameScreenController
4. **Buggy code:** this.stopTimer; is not running because it was missing parenthesis


### Confirming the fix

<!-- How did you confirm the problem is gone? -->
The results screen doen't continuously reload and the leaderboard is only updated once.

---

## Ticket 5: My high score disappeared from the leaderboard

### Actual cause

<!-- Which file and function was the bug in, and what was wrong with the code?
     Explain why that code produced the behavior described in the ticket. -->

**File / function:**
app.post("/api/leaderboard", (req, res)... in index.ts

**What was wrong:**
The entries were sort in ascending order with entries.sort((a, b) => a.score - b.score);

### Fix

<!-- What did you change, and why does that change fix the problem? -->
Switched a.score and b.score around to be entries.sort((a, b) => b.score - a.score); to sort in descending order.

### How did you find it?

#### Hypothesis testing

**What hypothesis ultimately led you to identify the buggy code, and why?**

I hypothesized that something was wrong with the sorting of the scores since the leaderboard was taking the lowest 5 scores instead of the top 5. So I went to index.ts and saw that it was sorting in ascending order instead of descending with entries.sort((a, b) => a.score - b.score);

### Confirming the fix

<!-- How did you confirm the problem is gone? -->
The list shows the five highest scores, highest first.