# 1. Project Title

# Hackathon Team Formation and Ranking System

A beginner-friendly Python console project that accepts hackathon teams and
student scores, validates every entry, calculates round-wise and overall team
scores, and ranks the teams using predefined tie-breaking rules.

---

# 2. Introduction

A hackathon is an event where teams of students build a project in a limited
time. Judges usually award marks over several rounds, and the organisers must
add up every student's marks, combine them into team totals, and publish a
leaderboard. Doing this by hand is slow and error-prone, especially when two
teams score exactly the same and a tie-breaker is needed.

This project solves that problem with a single Python script. The program
asks for the number of teams, the members of every team, and each student's
three round scores. It checks every input carefully (blank names, duplicate
names and IDs, wrong team sizes, and invalid scores are all rejected and
asked again), computes the round-wise totals, the overall total and the best
individual total of every team, sorts the teams **without using the built-in
`sort()` or `sorted()` functions**, assigns ranks with **competition ranking**
(the style `1, 2, 2, 4`), and prints a neatly aligned leaderboard and the
winner (or joint winners).

The project deliberately restricts itself to a beginner's syllabus:
computational thinking, algorithms, flowcharts, pseudocode, variables and
data types, operators, conditional statements, loops, strings, pattern
generation, lists and nested lists, and tuples (operations and unpacking).
Concepts such as functions, classes, dictionaries, sets, imports, file
handling, exception handling, list comprehensions, recursion and built-in
sorting are **not used anywhere** in the program.

---

# 3. Problem Statement

Organisers of a three-round hackathon need a program that:

1. Accepts the number of teams (at least two).
2. Accepts every team's name and member count, and every member's ID, name
   and three round scores — rejecting and re-asking every invalid entry
   **without** using exception handling.
3. Validates that each team has 2–4 members, team names are unique
   (ignoring letter case), and student IDs are unique across all teams.
4. Calculates, for each student, the total score across the three rounds.
5. Calculates, for each team, the Round 1 total, Round 2 total, Round 3
   total, overall total, and the highest individual student total in the
   team.
6. Ranks the teams by the rules given in Section 5, resolving ties with a
   fixed priority of tie-breakers and sharing ranks for exact ties.
7. Prints team compositions, individual scores, a formatted leaderboard and
   the winning team(s).

The core difficulty is to keep every team **correctly linked** to its scores
while sorting manually, and to implement **competition ranking** so that
exact ties share a rank and the numbering after a tie group skips correctly
(`1, 2, 2, 4`).

---

# 4. Objectives

- Read and validate all inputs using only loops, conditional statements,
  string methods and string checks — no `try/except`.
- Organise the data using only lists, nested lists and tuples.
- Demonstrate tuple unpacking while processing student records.
- Calculate student totals, team round totals, team overall totals and best
  individual totals using simple accumulation loops.
- Sort the teams manually with **selection sort** implemented through nested
  loops and conditional statements, swapping whole tuples so no team ever
  loses its scores.
- Assign competition ranks (`1, 2, 2, 4`) manually.
- Display a decorative heading, team compositions, individual score tables,
  an aligned leaderboard and the winner(s).
- Keep the entire program in one beginner-readable script.

---

# 5. Assumptions and Scoring Rules

> **Important:** The following rules are *proposed project rules framed for
> this project*. They are **not** rules supplied in the original question.

1. There are at least two teams.
2. Each team must contain 2 to 4 students.
3. Every student has a unique student ID.
4. A student can belong to only one team (the program collects each student
   exactly once, inside one team).
5. Team names must be unique, ignoring letter case (`Alpha` and `ALPHA` are
   treated as the same name).
6. The hackathon consists of three rounds.
7. Each student receives an integer score from 0 to 100 in each round.
8. A team's score in a round is the **sum** of its members' scores in that
   round, and the overall team total is the sum of the three round totals.
9. **Known limitation, stated openly:** because team scores are simple sums,
   bigger teams get more terms added, so a 4-member team has a natural
   advantage over a 2-member team. The project keeps this scoring method
   (as the rules specify) rather than silently switching to averages, and
   this limitation is discussed in Section 16.

**Ranking rules (checked in this exact order):**

| Priority | Rule |
|---|---|
| 1 | Higher overall team total ranks first |
| 2 | If tied, higher Round 3 total ranks first |
| 3 | If still tied, higher Round 2 total ranks first |
| 4 | If still tied, higher Round 1 total ranks first *(see note below)* |
| 5 | If still tied, higher best individual student total ranks first |
| 6 | If all of the above are equal, the teams **share the same rank** |

*Tie-break 4 note:* the Round 1 check is mathematically redundant. Since
`Overall = Round1 + Round2 + Round3`, two teams whose overall, Round 3 and
Round 2 totals are already equal **must** also have equal Round 1 totals.
The check can never change the result, but it is retained in the program
because the proposed rules list it explicitly. (Consistently, no test in
Section 15 claims that Round 1 alone resolved a tie — that can never
happen.)

Teams that share a rank are displayed alphabetically by name, ignoring
letter case. This only changes **display order**; the shared rank itself is
never broken. Ranks use **competition ranking**: teams at shared positions
receive the same rank number, and the next different team receives a rank
equal to its position, e.g. `1, 2, 2, 4`.

---

# 6. Input, Processing, and Output Analysis (IPO)

| Stage | Details |
|---|---|
| **Input** | Number of teams (whole number ≥ 2). For each team: name (non-blank, unique ignoring case), member count (whole number 2–4). For each member: ID (non-blank, unique across the event), name (non-blank), three round scores (whole numbers 0–100). All text is cleaned with `.strip()`. All numeric text is checked character-by-character against `"0123456789"` **before** `int()` conversion. |
| **Processing** | Accumulate each team's Round 1 / Round 2 / Round 3 totals; compute each student's total and track each team's best individual total; compute overall total = R1 + R2 + R3. Build one 7-field tuple per team. Manually selection-sort the tuples by the five ranking values (overall, R3, R2, R1, best individual), with alphabetical order as the final display tie-break. Assign competition ranks by comparing neighbouring sorted teams. |
| **Output** | Decorative heading (string repetition + pattern loop); team compositions with IDs, names, round scores and individual totals; final leaderboard with Rank, Team Name, Size, R1, R2, R3, Overall, Best Ind.; winner or joint winners. |

---

# 7. Computational Thinking

## 7.1 Decomposition (breaking the problem into smaller jobs)

1. Read and validate the number of teams.
2. For each team, read and validate the team name and size.
3. For each member, read and validate ID, name and three scores; store the
   record.
4. Calculate per-student totals and per-team round totals, overall totals
   and best individual totals.
5. Sort the teams by the ranking rules (manual selection sort).
6. Assign competition ranks.
7. Display compositions, leaderboard and winner(s).

Each sub-problem is small enough to be written as one clear loop.

## 7.2 Pattern recognition (things that repeat)

- **The validation pattern repeats everywhere:** strip the text → reject if
  blank → (for numbers) reject if any character is not a digit → convert
  with `int()` → reject if outside the allowed range. The same while-loop
  shape is reused for team count, team size and every score.
- **The uniqueness pattern repeats:** to check a new name/ID, scan the list
  of previously accepted values and compare (`.lower()` for names to ignore
  case).
- **The accumulation pattern repeats:** start totals at 0 and add each
  unpacked score — used for R1, R2, R3 totals and for finding the best
  individual.
- **The compare-and-decide pattern** in the sort: compare two teams value by
  value in priority order until one value decides.

## 7.3 Abstraction (what we keep and what we ignore)

- We **ignore** real-world details that do not affect ranking: judges,
  timings, venues, problem statements, student roll numbers of other kinds.
- A student is abstracted to a **5-field tuple**:
  `(id, name, round1, round2, round3)`.
- For ranking, a whole team is abstracted to a **7-field tuple** whose first
  five fields are exactly the five ranking values — so the sort only ever
  looks at what matters.
- Round labels are abstracted into one tuple `ROUND_LABELS`, used for both
  prompts and output headings.

## 7.4 Algorithm design (high-level strategy)

- **Sequential pipeline:** input → validate → organise → calculate → sort →
  rank → output.
- **Selection sort** is chosen for manual sorting: simple, uses only nested
  loops and conditionals, and swaps *whole tuples* so data stays linked.
- **Competition ranking** is achieved in one pass over the sorted list: the
  first team gets rank 1; each next team keeps the previous rank if all five
  ranking values match, otherwise it gets its position number (`i + 1`).
- **Defensive input design:** every numeric value is validated with string
  checks before conversion, so `int()` can never crash on bad text and no
  exception handling is needed.

---

# 8. Data Structures Used (with examples)

| Structure | Variable | Example contents | Why this structure |
|---|---|---|---|
| **Tuple (fixed labels)** | `ROUND_LABELS` | `("Round 1", "Round 2", "Round 3")` | The three labels never change; a tuple is immutable, which protects them and allows unpacking. |
| **List of strings** | `team_names` | `["Alpha", "beta", "Gamma", "Delta"]` | Team names in entry order; used for duplicate checking. |
| **List of integers** | `team_sizes` | `[3, 2, 4, 3]` | Member counts, index-linked to `team_names`. |
| **Nested list of tuples** | `all_teams` | `[ [("A101","Meera",100,95,95), ("A102","Rohan",40,45,60), ("A103","Sara",40,50,45)], [("B201","Kiran",100,90,100), ("B202","Divya",80,100,100)], ... ]` | Slot `t` holds one **inner list** per team; each inner item is an **immutable student record tuple** `(id, name, r1, r2, r3)`. Nested lists keep teams and their students perfectly linked. |
| **List of strings** | `all_student_ids` | `["A101", "A102", "A103", "B201", ...]` | Flat list for the event-wide duplicate-ID check (`in` operator). |
| **List of 7-field tuples** | `team_stats` | `(740, 260, 250, 230, 225, "Gamma", 4)` meaning `(overall, r3, r2, r1, best_individual, name, size)` | One tuple per team. The five ranking values come first, in rule priority order, so the sort compares them left-to-right. Swapping whole tuples during sorting keeps every team linked to its scores. |
| **List of integers** | `rank_list` | `[1, 2, 2, 4]` | `rank_list[i]` is the rank of `team_stats[i]`. |

**Why no dictionaries/sets?** They are outside the allowed syllabus. The
combination of parallel lists (`team_names` + `team_sizes`) and nested lists
of tuples covers everything needed, and the `in` operator on a list handles
membership tests.

---

# 9. Step-by-Step Algorithm

1. **Start.** Print a decorative heading (string repetition and a small
   pattern loop).
2. Set constants: `SMALLEST_EVENT = 2`, team size limits `2` and `4`, score
   limits `0` and `100`, `DIGITS = "0123456789"`, and the tuple
   `ROUND_LABELS = ("Round 1", "Round 2", "Round 3")`.
3. **Repeat** until valid: read the number of teams, strip spaces, reject if
   blank or if any character is not in `DIGITS`; convert with `int()`;
   reject if less than 2; otherwise accept.
4. Create empty lists `team_names`, `team_sizes`, `all_teams`,
   `all_student_ids`.
5. **For each team** `t = 1 to num_teams`:
   - **Repeat** until valid: read the team name, strip spaces, reject if
     blank; reject if it matches an existing name ignoring case
     (`name.lower()`); otherwise accept and append to `team_names`.
   - **Repeat** until valid: read the member count; apply the same
     whole-number string check; convert; reject if outside 2–4; otherwise
     append to `team_sizes`.
   - Create an empty inner list `team_students`.
   - **For each member** `m = 1 to team_size`:
     - **Repeat** until valid: read the student ID; reject if blank; reject
       if it is already in `all_student_ids`; otherwise accept.
     - **Repeat** until valid: read the student name; reject if blank.
     - **For each round** `r = 0 to 2`: **repeat** until valid: read the
       score; apply the whole-number string check; convert; reject if below
       0 or above 100; otherwise store it in `round_scores[r]`.
     - Build the record tuple
       `(student_id, student_name, r1, r2, r3)` and append it to
       `team_students`; append the ID to `all_student_ids`.
   - Append `team_students` to `all_teams`.
6. Create an empty list `team_stats`. **For each team**: set the three round
   totals and `best_individual` to `-1`; **for each student record**,
   *unpack* it into `student_id, student_name, s1, s2, s3`; add `s1, s2, s3`
   to the round totals; compute the student total; update
   `best_individual` if larger. Compute `overall = r1 + r2 + r3`; append the
   tuple `(overall, r3, r2, r1, best, name, size)` to `team_stats`.
7. **Manual selection sort** (descending by the five ranking values):
   - **For** `i = 0` to `n - 2`: assume position `i` holds the best
     (`max_index = i`).
   - **For** `j = i + 1` to `n - 1`: compare `team_stats[j]` with
     `team_stats[max_index]` field by field (`k = 0 … 4`) in priority order:
     if the challenger's field is greater it wins; if smaller it loses; if
     equal, move to the next field. If all five fields are equal, the team
     whose name comes first alphabetically (ignoring case) is placed first
     (display order only). Update `max_index` if the challenger is better.
   - Swap the **whole tuples** at `i` and `max_index`.
8. **Competition ranking:** create `rank_list`. Give the first team rank 1.
   **For** each later position `i`: compare its five ranking values with the
   previous team's; if all equal, copy the previous rank; otherwise assign
   rank `i + 1`.
9. **Display:** team compositions (each record unpacked to print ID, name,
   R1, R2, R3, total), then the leaderboard (each 7-field tuple unpacked),
   using fixed-width `format()` strings.
10. Collect the names of all teams with rank 1. If there is one, print
    `WINNER: name`; otherwise print `JOINT WINNERS:` and the names joined
    with `" and "`.
11. **End.**

---

# 10. Beginner-Friendly Pseudocode

```text
START
    DISPLAY heading made of "=" * 70 and a small star pattern

    REPEAT
        ASK number_of_teams
        IF blank OR any character not in "0123456789" THEN
            SHOW "invalid whole number"
        ELSE IF number_of_teams < 2 THEN
            SHOW "need at least 2 teams"
        END IF
    UNTIL number_of_teams is a whole number >= 2

    CREATE empty lists: team_names, team_sizes, all_teams, all_student_ids

    FOR each team t FROM 1 TO number_of_teams
        REPEAT
            ASK team_name (remove spaces from both ends)
            IF blank THEN SHOW "cannot be blank"
            ELSE IF team_name matches an earlier name IGNORING CASE THEN
                SHOW "already exists"
            END IF
        UNTIL team_name is accepted
        ADD team_name TO team_names

        REPEAT
            ASK team_size
            CHECK whole number (digits only) THEN CONVERT to integer
            IF team_size < 2 OR team_size > 4 THEN SHOW "must be 2 to 4"
        UNTIL team_size accepted
        ADD team_size TO team_sizes

        CREATE empty list team_students

        FOR each member m FROM 1 TO team_size
            REPEAT
                ASK student_id (remove spaces)
                IF blank THEN SHOW "cannot be blank"
                ELSE IF student_id already in all_student_ids THEN
                    SHOW "ID already used"
            UNTIL student_id accepted

            REPEAT
                ASK student_name (remove spaces)
                IF blank THEN SHOW "cannot be blank"
            UNTIL student_name accepted

            FOR each round r FROM 1 TO 3
                REPEAT
                    ASK score
                    CHECK whole number (digits only) THEN CONVERT
                    IF score < 0 OR score > 100 THEN SHOW "0 to 100 only"
                UNTIL score accepted
                STORE score IN round_scores[r]
            END FOR

            student_record ← (student_id, student_name,
                              round_scores[1], round_scores[2], round_scores[3])
            ADD student_record TO team_students
            ADD student_id TO all_student_ids
        END FOR

        ADD team_students TO all_teams        (nested list grows)
    END FOR

    CREATE empty list team_stats
    FOR each team t
        round1_total ← 0   round2_total ← 0   round3_total ← 0
        best_individual ← -1
        FOR each record IN all_teams[t]
            UNPACK record INTO (id, name, s1, s2, s3)      ' tuple unpacking
            round1_total ← round1_total + s1
            round2_total ← round2_total + s2
            round3_total ← round3_total + s3
            IF s1 + s2 + s3 > best_individual THEN
                best_individual ← s1 + s2 + s3
        END FOR
        overall ← round1_total + round2_total + round3_total
        ADD (overall, round3_total, round2_total, round1_total,
             best_individual, team_names[t], team_sizes[t]) TO team_stats
    END FOR

    ' ---- manual selection sort: best team first ----
    FOR i FROM 0 TO length(team_stats) - 2
        max_index ← i
        FOR j FROM i + 1 TO length(team_stats) - 1
            challenger ← team_stats[j]    champion ← team_stats[max_index]
            compare fields 0..4 LEFT TO RIGHT (overall, R3, R2, R1, best):
                first bigger field wins; equal fields continue
            IF all five fields equal THEN
                earlier alphabet (ignoring case) is placed first (display only)
            IF challenger is better THEN max_index ← j
        END FOR
        SWAP the WHOLE tuples team_stats[i] and team_stats[max_index]
    END FOR

    ' ---- competition ranking: 1, 2, 2, 4 ----
    rank_list ← empty
    FOR i FROM 0 TO length(team_stats) - 1
        IF i = 0 THEN rank ← 1
        ELSE IF all five ranking values equal to previous team THEN
            rank ← previous team's rank          ' shared rank
        ELSE rank ← i + 1                        ' gap after a tie group
        ADD rank TO rank_list
    END FOR

    DISPLAY team compositions with each record unpacked (ID, name, scores, total)
    DISPLAY leaderboard: Rank, Team Name, Size, R1, R2, R3, Overall, Best Ind.
    DISPLAY "WINNER" or "JOINT WINNERS" for every team whose rank = 1
END
```

---

# 11. Text-Based Flowchart

```text
                          +---------------------+
                          |        START        |
                          +----------+----------+
                                     |
                                     v
                  +--------------------------------------+
                  |   Print decorative heading           |
                  |   (string repetition + loop pattern) |
                  +------------------+-------------------+
                                     |
                                     v
              +---------------------------------------------+
     +------->|  INPUT number of teams                      |
     |        +----------------------+----------------------+
     |                               |
     |                               v
     |        [ blank OR non-digit character? OR count < 2? ]
     |            |                             |
     |           YES                            NO
     |            |                             |
     +----(show error, re-ask)                   |
     |                                           v
     |        +--------------------------------------------------+
     |        |  FOR each team t = 1 ... number_of_teams         |
     |        +----------------------+---------------------------+
     |                               |
     |                               v
     |        +---------------------------------------------+
     |        |  INPUT team name            .strip()        |
     |        +----------------------+----------------------+
     |                               v
     |        [ blank OR duplicate (case-insensitive)? ] --YES--> re-ask
     |                               | NO
     |                               v
     |        +---------------------------------------------+
     |        |  INPUT team size            .strip()        |
     |        +----------------------+----------------------+
     |                               v
     |        [ not all digits OR size outside 2..4?  ] --YES--> re-ask
     |                               | NO
     |                               v
     |        +---------------------------------------------+
     |        |  FOR each member m = 1 ... team_size        |
     |        +----------------------+----------------------+
     |                               v
     |        [ ID blank OR ID already used anywhere? ] --YES--> re-ask
     |                               | NO
     |                               v
     |        [ student name blank? ] ------------------YES--> re-ask
     |                               | NO
     |                               v
     |        +---------------------------------------------+
     |        |  FOR each round r = 1 ... 3                 |
     |        +----------------------+----------------------+
     |                               v
     |        [ not all digits OR score outside 0..100? ] -YES-> re-ask
     |                               | NO
     |                               v
     |        STORE tuple (id, name, s1, s2, s3) in the team's
     |        inner list; add the ID to all_student_ids
     |                               |
     |        (inner loops repeat until all members & rounds done)
     |                               |
     +--(back for the next team)-----+
                                     |
                                     v
        +-----------------------------------------------------------+
        |  CALCULATE (tuple unpacking of every record):             |
        |  per-team R1, R2, R3 totals, overall total,               |
        |  best individual total -> one 7-field tuple per team      |
        +----------------------------+------------------------------+
                                     |
                                     v
        +-----------------------------------------------------------+
        |  MANUAL SELECTION SORT of team_stats (NO sort/sorted):    |
        |  nested loops compare overall > R3 > R2 > R1 > best;      |
        |  swap WHOLE tuples so names never separate from scores    |
        +----------------------------+------------------------------+
                                     |
                                     v
        +-----------------------------------------------------------+
        |  RANKING: first team = rank 1; each next team keeps the   |
        |  previous rank if all 5 values match, else rank = position|
        |  (competition ranking: 1, 2, 2, 4)                        |
        +----------------------------+------------------------------+
                                     |
                                     v
        +-----------------------------------------------------------+
        |  OUTPUT: compositions + individual totals, formatted      |
        |  leaderboard, winner or JOINT WINNERS (rank 1 teams)      |
        +----------------------------+------------------------------+
                                     |
                                     v
                          +---------------------+
                          |         END         |
                          +---------------------+
```

---

# 12. Complete Executable Python Code

The same code is saved in the workspace as **`hackathon_team_ranking.py`**.

```python
"""
HACKATHON TEAM FORMATION AND RANKING SYSTEM
===========================================
A beginner-friendly console project kept in ONE script.

Built using ONLY these syllabus concepts:
  computational thinking, algorithms, flowcharts, pseudocode,
  variables and data types, operators, conditional statements, loops,
  strings, pattern generation, lists and nested lists,
  tuples, tuple operations and unpacking.

Deliberately avoided (as the restrictions require):
  user-defined functions / lambda, classes / OOP, dictionaries, sets,
  imports / external libraries, file handling, databases,
  exception handling, list comprehensions, recursion,
  and the built-in sort() / sorted() functions.
"""

# =====================================================================
# FIXED VALUES AND THE ROUND-LABELS TUPLE
# =====================================================================
WIDTH          = 70              # width of the decorative banners
DIGITS         = "0123456789"    # allowed characters for whole-number input
KEY_PARTS      = 5               # ranking values kept for every team

SMALLEST_EVENT = 2               # assumption 1: at least two teams
SMALLEST_TEAM  = 2               # assumption 2: team size lower limit
LARGEST_TEAM   = 4               # assumption 2: team size upper limit
LOWEST_SCORE   = 0               # assumption 7: score range
HIGHEST_SCORE  = 100

# ROUND_LABELS is a TUPLE: the three round names never change, so an
# immutable sequence is exactly the right container for them.
ROUND_LABELS = ("Round 1", "Round 2", "Round 3")

# =====================================================================
# DECORATIVE HEADING (string repetition + a small pattern loop)
# =====================================================================
print("=" * WIDTH)
print("HACKATHON TEAM FORMATION AND RANKING SYSTEM".center(WIDTH))
print("=" * WIDTH)
print("3 rounds | scores 0-100 | team size 2-4 | highest total wins".center(WIDTH))
print()
for stars in range(1, 4):                    # tiny pattern: 1, 3, 5 stars
    print(("*" * (2 * stars - 1)).center(WIDTH))
print()

# =====================================================================
# STAGE 1 : INPUT AND VALIDATION
# =====================================================================

# ---- 1A. number of teams (whole number, at least SMALLEST_EVENT) ----
num_teams = 0
teams_ok = False
while teams_ok == False:
    typed = input("Enter the number of teams (minimum " + str(SMALLEST_EVENT) + ") : ").strip()
    is_whole = True                          # string check BEFORE int()
    if len(typed) == 0:
        is_whole = False
    else:
        for one_char in typed:
            if one_char not in DIGITS:
                is_whole = False
    if is_whole == False:
        print("   INVALID! Type a whole number, e.g. 3 (no letters or symbols).")
    else:
        num_teams = int(typed)               # safe type conversion
        if num_teams < SMALLEST_EVENT:
            print("   INVALID! This event needs at least " + str(SMALLEST_EVENT) + " teams.")
        else:
            teams_ok = True

# ---------------------------------------------------------------------
# MAIN DATA STRUCTURES
#
# team_names       -> LIST of text values      e.g. ["Alpha", "beta"]
# team_sizes       -> LIST of whole numbers    e.g. [3, 2]
# all_teams        -> NESTED LIST. Slot t holds a LIST of that team's
#                     student RECORDS, and every record is a TUPLE:
#                         (id, name, round1, round2, round3)
#                     e.g. [ [("A101","Meera",100,95,95), ("A102",...)],
#                            [("B201",...)] ]
# all_student_ids  -> flat LIST of every ID used so far (duplicate check)
# ---------------------------------------------------------------------
team_names      = []
team_sizes      = []
all_teams       = []
all_student_ids = []

# ---- 1B. details of every team and its students ---------------------
for t in range(num_teams):
    print()
    print("-" * WIDTH)
    print("TEAM " + str(t + 1) + " OF " + str(num_teams))
    print("-" * WIDTH)

    # team name: not blank, unique ignoring letter case (assumption 5)
    team_name = ""
    name_ok = False
    while name_ok == False:
        team_name = input("Team name                        : ").strip()
        if len(team_name) == 0:
            print("   INVALID! Team name cannot be blank.")
        else:
            already_used = False
            for name_seen in team_names:
                if name_seen.lower() == team_name.lower():
                    already_used = True
            if already_used == True:
                print("   INVALID! '" + team_name + "' already exists (letter case is ignored).")
            else:
                name_ok = True
    team_names.append(team_name)

    # team size: whole number between 2 and 4 (assumption 2)
    team_size = 0
    size_ok = False
    while size_ok == False:
        typed = input("Number of members (2 to 4)       : ").strip()
        is_whole = True
        if len(typed) == 0:
            is_whole = False
        else:
            for one_char in typed:
                if one_char not in DIGITS:
                    is_whole = False
        if is_whole == False:
            print("   INVALID! Type a whole number, e.g. 3.")
        else:
            team_size = int(typed)
            if team_size < SMALLEST_TEAM or team_size > LARGEST_TEAM:
                print("   INVALID! A team must have " + str(SMALLEST_TEAM) + " to " + str(LARGEST_TEAM) + " members.")
            else:
                size_ok = True
    team_sizes.append(team_size)

    # students of this team
    team_students = []      # LIST of student TUPLES for this team only
    print()
    print("Enter the " + str(team_size) + " member(s) of team '" + team_name + "':")
    for m in range(team_size):
        print(" Member " + str(m + 1) + " of " + str(team_size) + ":")

        # student ID: not blank, unique across ALL teams (assumptions 3, 4)
        student_id = ""
        id_ok = False
        while id_ok == False:
            student_id = input("   Student ID                    : ").strip()
            if len(student_id) == 0:
                print("   INVALID! Student ID cannot be blank.")
            elif student_id in all_student_ids:
                print("   INVALID! ID '" + student_id + "' is already used by another student.")
            else:
                id_ok = True

        # student name: not blank
        student_name = ""
        sname_ok = False
        while sname_ok == False:
            student_name = input("   Student name                  : ").strip()
            if len(student_name) == 0:
                print("   INVALID! Student name cannot be blank.")
            else:
                sname_ok = True

        # three round scores: whole numbers in 0..100 (assumption 7)
        round_scores = [0, 0, 0]             # LIST holding the 3 scores
        for r in range(3):
            score_ok = False
            while score_ok == False:
                typed = input("   " + ROUND_LABELS[r] + " score (0-100)         : ").strip()
                is_whole = True
                if len(typed) == 0:
                    is_whole = False
                else:
                    for one_char in typed:
                        if one_char not in DIGITS:
                            is_whole = False
                if is_whole == False:
                    print("   INVALID! Scores must be whole numbers, e.g. 85.")
                else:
                    score_value = int(typed)
                    if score_value < LOWEST_SCORE or score_value > HIGHEST_SCORE:
                        print("   INVALID! Score must be between 0 and 100.")
                    else:
                        score_ok = True
                        round_scores[r] = score_value

        # a FIXED student record -> stored as a TUPLE (immutable).
        # Tuple CONCATENATION joins the (id, name) pair with the scores.
        student_record = (student_id, student_name) + (round_scores[0], round_scores[1], round_scores[2])
        team_students.append(student_record)
        all_student_ids.append(student_id)

    all_teams.append(team_students)

# =====================================================================
# STAGE 2 : SCORE CALCULATIONS
# =====================================================================
# team_stats is a LIST of TUPLES, one tuple per team:
#   ( overall_total, round3_total, round2_total, round1_total,
#     best_individual_total, team_name, team_size )
# The five ranking values come FIRST so the manual sort can compare
# them in the exact priority order of the proposed rules. Keeping ALL
# values in ONE tuple means a swap during sorting moves the whole team
# as a unit, so a team name can never get separated from its scores.
# ---------------------------------------------------------------------
team_stats = []
for t in range(num_teams):
    round1_total = 0
    round2_total = 0
    round3_total = 0
    best_individual = -1                  # -1 is impossible, so the first
    for record in all_teams[t]:           # student's total always replaces it
        # TUPLE UNPACKING: the 5-field record opens into 5 variables
        student_id, student_name, s1, s2, s3 = record
        round1_total = round1_total + s1
        round2_total = round2_total + s2
        round3_total = round3_total + s3
        student_total = s1 + s2 + s3
        if student_total > best_individual:
            best_individual = student_total
    overall_total = round1_total + round2_total + round3_total
    one_team = (overall_total, round3_total, round2_total, round1_total,
                best_individual, team_names[t], team_sizes[t])
    team_stats.append(one_team)

# =====================================================================
# STAGE 3 : MANUAL SORTING (selection sort - NO sort() / sorted())
# =====================================================================
# REDUNDANCY NOTE (kept because the proposed rules list it):
# Overall = Round1 + Round2 + Round3. So if the overall, Round 3 and
# Round 2 totals are already equal, the Round 1 totals MUST be equal
# too. The Round 1 check can therefore never decide anything by
# itself, but it is still written here because the rules ask for it.
# ---------------------------------------------------------------------
num_stats = len(team_stats)
for i in range(num_stats - 1):
    max_index = i
    for j in range(i + 1, num_stats):
        challenger = team_stats[j]         # team that may be better
        champion  = team_stats[max_index]  # best team found so far
        challenger_better = False
        decided = False
        k = 0
        while k < KEY_PARTS and decided == False:   # priority order
            if challenger[k] > champion[k]:
                challenger_better = True
                decided = True
            elif challenger[k] < champion[k]:
                decided = True
            k = k + 1
        if decided == False:
            # all five ranking values equal -> alphabetical, ignore case.
            # This only fixes the DISPLAY order of shared ranks; it can
            # never turn a shared rank into different ranks.
            if challenger[5].lower() < champion[5].lower():
                challenger_better = True
        if challenger_better == True:
            max_index = j
    if max_index != i:
        # swap the WHOLE tuples so every team stays linked to its scores
        temp = team_stats[i]
        team_stats[i] = team_stats[max_index]
        team_stats[max_index] = temp

# =====================================================================
# STAGE 4 : RANKING (competition ranking: 1, 2, 2, 4 ...)
# =====================================================================
rank_list = []                # rank_list[i] is the rank of team_stats[i]
for i in range(num_stats):
    if i == 0:
        rank_list.append(1)
    else:
        earlier = team_stats[i - 1]
        current = team_stats[i]
        same_values = True
        for k in range(KEY_PARTS):
            if earlier[k] != current[k]:
                same_values = False
        if same_values == True:
            rank_list.append(rank_list[i - 1])   # share the earlier rank
        else:
            rank_list.append(i + 1)              # gap after a tie group

# =====================================================================
# STAGE 5 : OUTPUT
# =====================================================================
# Reusable format strings (string methods keep the columns aligned)
COMP_ROW = "    {:<10} {:<15} {:>8} {:>8} {:>8} {:>8}"
LEAD_ROW = "{:<5} {:<20} {:>4} {:>5} {:>5} {:>5} {:>8} {:>10}"

print()
print("=" * WIDTH)
print("RESULTS".center(WIDTH))
print("=" * WIDTH)

# ---- part 1: team compositions and individual scores ----------------
print()
print("TEAM COMPOSITIONS AND INDIVIDUAL SCORES")
print("-" * WIDTH)
for t in range(num_teams):
    print()
    print("Team '" + team_names[t] + "'  (size " + str(team_sizes[t]) + ")")
    print(COMP_ROW.format("ID", "Name", "R1", "R2", "R3", "Total"))
    for record in all_teams[t]:
        student_id, student_name, s1, s2, s3 = record   # tuple unpacking
        student_total = s1 + s2 + s3
        print(COMP_ROW.format(student_id, student_name, s1, s2, s3, student_total))

# ---- part 2: leaderboard --------------------------------------------
print()
print()
print("FINAL LEADERBOARD")
print("-" * WIDTH)
print(LEAD_ROW.format("Rank", "Team Name", "Size", "R1", "R2", "R3", "Overall", "Best Ind."))
print("-" * WIDTH)
for i in range(num_stats):
    # unpacking the 7-field team tuple
    overall, r3t, r2t, r1t, best_i, name_i, size_i = team_stats[i]
    shown_name = name_i
    if len(shown_name) > 20:
        shown_name = shown_name[:17] + "..."   # SLICING keeps columns tidy
    print(LEAD_ROW.format(rank_list[i], shown_name, size_i, r1t, r2t, r3t, overall, best_i))
print("-" * WIDTH)
print("(Ranks use competition style: exact ties share a rank, e.g. 1, 2, 2, 4)")

# ---- part 3: winner(s) ----------------------------------------------
champion_names = []
for i in range(num_stats):
    if rank_list[i] == 1:
        champion_names.append(team_stats[i][5])
print()
if len(champion_names) == 1:
    print("***  WINNER: " + champion_names[0] + "  ***")
else:
    line = "JOINT WINNERS: "
    for idx in range(len(champion_names)):
        if idx > 0:
            line = line + " and "
        line = line + champion_names[idx]
    print("***  " + line + "  ***")
print()
print("=" * WIDTH)
print("Thank you! Ranking completed successfully.".center(WIDTH))
print("=" * WIDTH)
```

---

# 13. Explanation of the Important Code Sections

**1. Decorative heading (string repetition + pattern loop).**
`print("=" * WIDTH)` uses string repetition, and the small `for stars in
range(1, 4)` loop prints centred rows of 1, 3 and 5 stars — a tiny pattern
generated with a loop, as the requirements ask.

**2. Whole-number validation without exception handling.**
Every numeric input runs through the same pattern: `.strip()` removes
leading/trailing spaces, an empty string is rejected, and a `for` loop scans
every character against `DIGITS = "0123456789"`. Only after this string
check passes is `int(typed)` called, so the conversion can never crash.
Inputs like `"abc"`, `"3.5"`, `"-5"` and blanks are all rejected safely and
the `while` loop simply asks again — no `try/except` anywhere.

**3. Duplicate checks.**
Team names are compared with `.lower()` on both sides, so `"ALPHA"` is
rejected when `"Alpha"` already exists (Assumption 5). Student IDs are
collected in the flat list `all_student_ids` and the `in` operator tests
membership across **all** teams, not just the current one (Assumptions 3
and 4).

**4. Tuples for fixed records + tuple operations.**
Each student is stored as the immutable tuple
`(student_id, student_name) + (r1, r2, r3)` — **tuple concatenation** joins
the identity pair with the score triple. Immutability protects the record
from accidental change, which is exactly why a tuple (not a list) is the
right container.

**5. Tuple unpacking.**
`student_id, student_name, s1, s2, s3 = record` unpacks each 5-field record
in the calculation stage (and again when printing compositions). The
7-field team tuple is likewise unpacked when printing the leaderboard:
`overall, r3t, r2t, r1t, best_i, name_i, size_i = team_stats[i]`.

**6. Why the 7-field tuple keeps data linked during sorting.**
`team_stats` stores one tuple per team in the form
`(overall, r3, r2, r1, best, name, size)`. Because the whole tuple is
swapped in one go, a team's name and size can never drift away from its
scores. The five ranking fields are placed first, in rule priority order,
so the sort reads them left to right — exactly the order of the rules.

**7. Manual selection sort (no `sort()`/`sorted()`).**
The outer loop grows a sorted prefix; the inner loop scans the rest for the
best team. The `while k < KEY_PARTS` loop compares field `k` of the two
tuples: greater → challenger wins, smaller → champion stands, equal → check
the next field. Only if **all five** fields are equal does the name decide —
alphabetically ignoring case — which merely orders shared ranks for
display and never splits a shared rank. The **Round 1 redundancy note**
inside this section explains that the Round 1 comparison can never decide
anything (because equal overall + R3 + R2 forces equal R1), but the check
is retained because the proposed rules list it.

**8. Competition ranking.**
The first team gets rank 1. For every later position, the team's five
ranking values are compared with the previous team's: all equal → it copies
the previous rank (shared rank); any difference → it gets `i + 1`, its
position number. This single rule produces `1, 2, 2, 4` automatically.

**9. Alignment and slicing.**
Reusable format strings (`COMP_ROW`, `LEAD_ROW`) with `{:>8}`-style
placeholders keep every column neatly aligned. If a team name is longer
than 20 characters, slicing (`shown_name[:17] + "..."`) trims it so the
table never breaks.

---

# 14. Sample Input/Output Demonstration

The demonstration below is a **real, verified run** of the program with the
inputs shown. It produces ranks `1, 2, 2, 4` — including an exact tie
(shared rank 2, displayed alphabetically) — and every total can be checked
by hand.

**Input given (in order):**

```text
4
Alpha   -> members: A101 Meera 100 95 95 | A102 Rohan 40 45 60 | A103 Sara 40 50 45
beta    -> members: B201 Kiran 100 90 100 | B202 Divya 80 100 100
Gamma   -> members: G301 Aditya 70 75 80 | G302 Nandini 60 65 70
                     G303 Farhan 55 60 65 | G304 Priya 45 50 45
Delta   -> members: D401 Yash 60 65 70 | D402 Ira 55 60 65 | D403 Kunal 45 50 50
```

**Program session (prompts and typed answers):**

```text
======================================================================
             HACKATHON TEAM FORMATION AND RANKING SYSTEM
======================================================================
          3 rounds | scores 0-100 | team size 2-4 | highest total wins

                                  *
                                 ***
                                *****

Enter the number of teams (minimum 2) : 4

----------------------------------------------------------------------
TEAM 1 OF 4
----------------------------------------------------------------------
Team name                        : Alpha
Number of members (2 to 4)       : 3

Enter the 3 member(s) of team 'Alpha':
 Member 1 of 3:
   Student ID                    : A101
   Student name                  : Meera
   Round 1 score (0-100)         : 100
   Round 2 score (0-100)         : 95
   Round 3 score (0-100)         : 95
 Member 2 of 3:
   Student ID                    : A102
   Student name                  : Rohan
   Round 1 score (0-100)         : 40
   Round 2 score (0-100)         : 45
   Round 3 score (0-100)         : 60
 Member 3 of 3:
   Student ID                    : A103
   Student name                  : Sara
   Round 1 score (0-100)         : 40
   Round 2 score (0-100)         : 50
   Round 3 score (0-100)         : 45

----------------------------------------------------------------------
TEAM 2 OF 4
----------------------------------------------------------------------
Team name                        : beta
Number of members (2 to 4)       : 2

Enter the 2 member(s) of team 'beta':
 Member 1 of 2:
   Student ID                    : B201
   Student name                  : Kiran
   Round 1 score (0-100)         : 100
   Round 2 score (0-100)         : 90
   Round 3 score (0-100)         : 100
 Member 2 of 2:
   Student ID                    : B202
   Student name                  : Divya
   Round 1 score (0-100)         : 80
   Round 2 score (0-100)         : 100
   Round 3 score (0-100)         : 100

----------------------------------------------------------------------
TEAM 3 OF 4
----------------------------------------------------------------------
Team name                        : Gamma
Number of members (2 to 4)       : 4

Enter the 4 member(s) of team 'Gamma':
 Member 1 of 4:
   Student ID                    : G301
   Student name                  : Aditya
   Round 1 score (0-100)         : 70
   Round 2 score (0-100)         : 75
   Round 3 score (0-100)         : 80
 Member 2 of 4:
   Student ID                    : G302
   Student name                  : Nandini
   Round 1 score (0-100)         : 60
   Round 2 score (0-100)         : 65
   Round 3 score (0-100)         : 70
 Member 3 of 4:
   Student ID                    : G303
   Student name                  : Farhan
   Round 1 score (0-100)         : 55
   Round 2 score (0-100)         : 60
   Round 3 score (0-100)         : 65
 Member 4 of 4:
   Student ID                    : G304
   Student name                  : Priya
   Round 1 score (0-100)         : 45
   Round 2 score (0-100)         : 50
   Round 3 score (0-100)         : 45

----------------------------------------------------------------------
TEAM 4 OF 4
----------------------------------------------------------------------
Team name                        : Delta
Number of members (2 to 4)       : 3

Enter the 3 member(s) of team 'Delta':
 Member 1 of 3:
   Student ID                    : D401
   Student name                  : Yash
   Round 1 score (0-100)         : 60
   Round 2 score (0-100)         : 65
   Round 3 score (0-100)         : 70
 Member 2 of 3:
   Student ID                    : D402
   Student name                  : Ira
   Round 1 score (0-100)         : 55
   Round 2 score (0-100)         : 60
   Round 3 score (0-100)         : 65
 Member 3 of 3:
   Student ID                    : D403
   Student name                  : Kunal
   Round 1 score (0-100)         : 45
   Round 2 score (0-100)         : 50
   Round 3 score (0-100)         : 50
======================================================================
                               RESULTS
======================================================================

TEAM COMPOSITIONS AND INDIVIDUAL SCORES
----------------------------------------------------------------------

Team 'Alpha'  (size 3)
    ID         Name                  R1       R2       R3    Total
    A101       Meera                100       95       95      290
    A102       Rohan                 40       45       60      145
    A103       Sara                  40       50       45      135

Team 'beta'  (size 2)
    ID         Name                  R1       R2       R3    Total
    B201       Kiran                100       90      100      290
    B202       Divya                 80      100      100      280

Team 'Gamma'  (size 4)
    ID         Name                  R1       R2       R3    Total
    G301       Aditya                70       75       80      225
    G302       Nandini               60       65       70      195
    G303       Farhan                55       60       65      180
    G304       Priya                 45       50       45      140

Team 'Delta'  (size 3)
    ID         Name                  R1       R2       R3    Total
    D401       Yash                  60       65       70      195
    D402       Ira                   55       60       65      180
    D403       Kunal                 45       50       50      145


FINAL LEADERBOARD
----------------------------------------------------------------------
Rank  Team Name            Size    R1    R2    R3  Overall  Best Ind.
----------------------------------------------------------------------
1     Gamma                   4   230   250   260      740        225
2     Alpha                   3   180   190   200      570        290
2     beta                    2   180   190   200      570        290
4     Delta                   3   160   175   185      520        195
----------------------------------------------------------------------
(Ranks use competition style: exact ties share a rank, e.g. 1, 2, 2, 4)

***  WINNER: Gamma  ***

======================================================================
              Thank you! Ranking completed successfully.
======================================================================
```

**Hand-verification of the leaderboard (every number checks out):**

- **Gamma:** R1 = 70+60+55+45 = **230**; R2 = 75+65+60+50 = **250**;
  R3 = 80+70+65+45 = **260**; Overall = **740**; Best individual = 70+75+80 = **225**.
- **Alpha:** R1 = 100+40+40 = **180**; R2 = 95+45+50 = **190**;
  R3 = 95+60+45 = **200**; Overall = **570**; Best = 100+95+95 = **290**.
- **beta:** R1 = 100+80 = **180**; R2 = 90+100 = **190**; R3 = 100+100 = **200**;
  Overall = **570**; Best = 100+90+100 = **290**.
- **Delta:** R1 = 60+55+45 = **160**; R2 = 65+60+50 = **175**;
  R3 = 70+65+50 = **185**; Overall = **520**; Best = 60+65+70 = **195**.

Alpha and beta match on **all five** ranking values, so they share rank 2
and are displayed alphabetically (`Alpha` before `beta`, ignoring case).
Delta therefore receives rank **4**, not 3 — exactly the `1, 2, 2, 4`
competition pattern.

---

# 15. Test-Case Table

All expected results below were **verified by actually running the
program** (the tie scenarios are reproduced in the `tests/` folder with
ready-made input files).

| # | Test | Input given | Expected result | Verified outcome |
|---|------|-------------|-----------------|------------------|
| 1 | Valid teams and scores | 4 teams (Section 14 data) | Correct totals and ranks `1, 2, 2, 4`; winner Gamma | ✔ Pass — totals 740 / 570 / 570 / 520, ranks 1, 2, 2, 4 |
| 2 | Number of teams below minimum | `1`, then `0` | Rejected, asked again | ✔ Pass — "This event needs at least 2 teams." |
| 3 | Non-numeric count | `abc`, then `3.5` | Rejected by digit check before `int()` | ✔ Pass — "Type a whole number..." |
| 4 | Too few members | team size `1` | Rejected, asked again | ✔ Pass — "A team must have 2 to 4 members." |
| 5 | Too many members | team size `5` | Rejected, asked again | ✔ Pass — "A team must have 2 to 4 members." |
| 6 | Non-numeric size | `two` | Rejected | ✔ Pass — "Type a whole number, e.g. 3." |
| 7 | Non-numeric / decimal score | `abc`, then `88.5` | Rejected by digit check (`.` is not a digit) | ✔ Pass — "Scores must be whole numbers..." |
| 8 | Score above 100 | `105` | Passes digit check, rejected by range check | ✔ Pass — "Score must be between 0 and 100." |
| 9 | Negative score | `-5` | The `-` fails the digit check → rejected as not a whole number | ✔ Pass — "Scores must be whole numbers..." |
| 10 | Duplicate team name, different case | team named `Alpha`, then `alpha` offered again | Second one rejected (case-insensitive) | ✔ Pass — "'alpha' already exists (letter case is ignored)." |
| 11 | Duplicate student ID across teams | `S01` used in team 1, reused in team 1's second member | Reused ID rejected | ✔ Pass — "ID 'S01' is already used by another student." |
| 12 | Blank team name / ID / student name | spaces-only `"   "` and an empty line | All rejected after `.strip()` | ✔ Pass — "...cannot be blank." for each |
| 13 | Boundary scores accepted | `0`, `100` | Accepted (range is inclusive) | ✔ Pass — stored correctly |
| 14 | Tie resolved by **Round 3** | TeamX: Ariv 60/55/55, Diwan 80/70/80 → totals 140/125/135 = **400**; TeamY: Ishan 50/65/70, Jatin 75/60/80 → totals 125/125/150 = **400** | Overall tied (400); R3: 150 > 135 → TeamY rank 1, TeamX rank 2 | ✔ Pass — leaderboard shows TeamY (1) above TeamX (2) |
| 15 | Tie resolved by **Round 2** | TeamP: Karan 65/70/60, Lata 60/55/70 → 125/125/130 = **380**; TeamQ: Manav 70/60/65, Neha 50/70/65 → 120/130/130 = **380** | Overall tied; R3 tied (130 = 130); R2: 130 > 125 → TeamQ rank 1, TeamP rank 2 | ✔ Pass — TeamQ (1), TeamP (2) |
| 16 | Tie resolved by **best individual** | TeamM: Omkar 80/90/100, Rhea 70/70/70 → 150/160/170 = **480**, best 270; TeamN: Sana 75/80/85, Tarun 75/80/85 → 150/160/170 = **480**, best 240 | Overall, R3, R2, R1 all tied; best individual 270 > 240 → TeamM rank 1, TeamN rank 2 | ✔ Pass — TeamM (1), TeamN (2) |
| 17 | Exact tie → **shared ranks** | Section 14: Alpha and beta identical on all five values | Both get rank 2, shown alphabetically (`Alpha`, then `beta`) | ✔ Pass — ranks 2 and 2 |
| 18 | Correct numbering **after** a shared rank | Section 14 leaderboard | Delta, positioned 4th, gets rank **4** (not 3) | ✔ Pass — `1, 2, 2, 4` |
| 19 | Joint winners displayed | TeamM (150/160/170, best 240) and TeamN (150/160/170, best 240) — all five values equal | Ranks 1 and 1; output "JOINT WINNERS: TeamM and TeamN" (alphabetical) | ✔ Pass |

**Note on the Round 1 tie-break (as required):** no test above claims that
Round 1 alone resolved a tie — that is mathematically impossible, because
equal overall, Round 3 and Round 2 totals force equal Round 1 totals
(`Overall = R1 + R2 + R3`). The Round 1 comparison exists in the code only
because the proposed rules list it explicitly, and Tests 14–16 show the
real deciding levels (R3, R2, best individual).

**Validation run transcript (evidence for tests 2–12):**

```text
Enter the number of teams (minimum 2) : 1
   INVALID! This event needs at least 2 teams.
Enter the number of teams (minimum 2) : 0
   INVALID! This event needs at least 2 teams.
Enter the number of teams (minimum 2) : abc
   INVALID! Type a whole number, e.g. 3 (no letters or symbols).
Enter the number of teams (minimum 2) : 2

----------------------------------------------------------------------
TEAM 1 OF 2
----------------------------------------------------------------------
Team name                        :
   INVALID! Team name cannot be blank.
Team name                        : Alpha
Number of members (2 to 4)       : 1
   INVALID! A team must have 2 to 4 members.
Number of members (2 to 4)       : 5
   INVALID! A team must have 2 to 4 members.
Number of members (2 to 4)       : two
   INVALID! Type a whole number, e.g. 3.
Number of members (2 to 4)       : 3

Enter the 3 member(s) of team 'Alpha':
 Member 1 of 3:
   Student ID                    :
   INVALID! Student ID cannot be blank.
   Student ID                    : S01
   Student name                  :
   INVALID! Student name cannot be blank.
   Student name                  : Ravi
   Round 1 score (0-100)         : abc
   INVALID! Scores must be whole numbers, e.g. 85.
   Round 1 score (0-100)         : 105
   INVALID! Score must be between 0 and 100.
   Round 1 score (0-100)         : -5
   INVALID! Scores must be whole numbers, e.g. 85.
   Round 1 score (0-100)         : 85
   Round 2 score (0-100)         : 90
   Round 3 score (0-100)         : 80
 Member 2 of 3:
   Student ID                    : S01
   INVALID! ID 'S01' is already used by another student.
   Student ID                    : S02
   Student name                  : Meena
   Round 1 score (0-100)         : 70
   Round 2 score (0-100)         : 65
   Round 3 score (0-100)         : 75
 Member 3 of 3:
   Student ID                    : S03
   Student name                  : Farah
   Round 1 score (0-100)         : 88
   Round 2 score (0-100)         : 92
   Round 3 score (0-100)         : 79

----------------------------------------------------------------------
TEAM 2 OF 2
----------------------------------------------------------------------
Team name                        : alpha
   INVALID! 'alpha' already exists (letter case is ignored).
Team name                        : beta
Number of members (2 to 4)       : 2

      ... (members of beta entered normally) ...

FINAL LEADERBOARD
----------------------------------------------------------------------
Rank  Team Name            Size    R1    R2    R3  Overall  Best Ind.
----------------------------------------------------------------------
1     Alpha                   3   243   247   234      724        259
2     beta                    2   182   137   157      476        248
----------------------------------------------------------------------

***  WINNER: Alpha  ***
```

(Hand-check: Alpha R1 = 85+70+88 = 243, R2 = 90+65+92 = 247,
R3 = 80+75+79 = 234, Overall = 724, Best = 259 — Farah;
beta R1 = 94+88 = 182, R2 = 61+76 = 137, R3 = 73+84 = 157,
Overall = 476, Best = 248 — Vijay.)

---

# 16. Limitations and Possible Future Improvements

**Limitations**

1. **Team-size bias (acknowledged, by design):** scores are summed, so a
   4-member team adds four numbers per round while a 2-member team adds
   two. Bigger teams therefore tend to beat smaller teams even with weaker
   individual performances. The proposed rules keep sum-based scoring, and
   the program does not silently "fix" this.
2. Only whole-number scores in 0–100 are accepted; decimal or weighted
   marks are not supported.
3. The number of rounds (3) and size limits (2–4) are fixed constants;
   changing them means editing the constants and, for rounds, the stored
   record shape.
4. Because user-defined functions are outside the syllabus, the validation
   block is repeated at each input point, making the script longer than a
   function-based version.
5. Data lives only in memory — nothing is saved, and a closed session must
   be re-typed. Names longer than 20 characters are trimmed with `...` in
   the leaderboard.
6. Teams that tie on **all five** values share a rank forever; no further
   criterion (e.g., a sudden-death round) exists.

**Possible future improvements**

1. Score teams by **average per member** (or normalised scores) to remove
   the team-size bias — with the change announced clearly, not hidden.
2. Move the repeated validation into reusable user-defined functions once
   functions are in the syllabus.
3. Use dictionaries (`id -> record`) for O(1) duplicate-ID lookups and
   cleaner data access.
4. Allow a configurable number of rounds by reading `ROUND_LABELS` length
   dynamically.
5. Save results to a CSV/text file and reload them (file handling).
6. Add judge-weighted rounds, a GUI or web interface, and export of the
   leaderboard.

---

# 17. Five Short Viva Questions with Answers

**Q1. Why are student records stored as tuples instead of lists?**
A student's record `(id, name, r1, r2, r3)` is fixed data that should not
change after entry. A tuple is immutable, so it protects the record from
accidental modification, and it can be unpacked directly into five
variables, which makes processing clean and readable.

**Q2. Why is manual sorting used instead of `sort()` or `sorted()`?**
Because the syllabus forbids built-in sorting. Selection sort — nested
loops plus conditional statements — is implemented manually. Comparing
whole 7-field tuples (and swapping them whole) also guarantees that a
team's name and size always stay attached to its scores.

**Q3. Why can the Round 1 tie-break never actually decide a winner, and why
is it still in the code?**
Because `Overall = Round1 + Round2 + Round3`. If two teams already match on
overall, Round 3 and Round 2 totals, their Round 1 totals must be equal
too — the subtraction is forced. So the Round 1 comparison can never break
a tie by itself. It is retained because the proposed ranking rules list it
explicitly, and the code comments state this openly.

**Q4. How does the program produce competition ranking like 1, 2, 2, 4?**
After sorting, the first team gets rank 1. Each later team compares its
five ranking values with the team directly above it: if all five match, it
copies that team's rank (shared rank); otherwise it gets its own position
number (`i + 1`). So after a pair sharing rank 2, the next team gets rank
4 — the numbering skips, which is exactly competition ranking.

**Q5. How is invalid numeric input rejected without exception handling?**
Every number is validated as **text first**: leading/trailing spaces are
stripped, an empty entry is rejected, and a loop checks that every single
character appears in `"0123456789"`. Only then is `int()` called, so the
conversion can never fail. Range checks (0–100, 2–4 members, ≥ 2 teams)
run afterwards, and the surrounding `while` loop simply re-asks until the
entry is valid.

---

*End of report. Files in the workspace: `hackathon_team_ranking.py` (the
program), `Hackathon_Team_Ranking_Report.md` (this report),
`sample_input.txt` (replays the Section 14 demo via
`python3 hackathon_team_ranking.py < sample_input.txt`), and the `tests/`
folder with the tie-scenario input files (`tie_round3.txt`,
`tie_round2.txt`, `tie_best_individual.txt`, `tie_joint_winners.txt`),
a validation run (`validation_demo.txt`) and a decimal-input run
(`decimal_input.txt`).*
