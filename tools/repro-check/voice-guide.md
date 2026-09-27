# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

I do API and UI test automation for a living (TypeScript, Postman,
Newman) and this is my first open-source contribution. In Path Review
I am working a docs issue: example `curl` commands for `docs/API.md`.
Readers can expect me to show what I ran and what came back, say what
I did not check, and promise only the next step I will actually take.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: Promise the investigation, not the fix

I commit to steps I control (read, run, compare, report back). I never
name a date and I never guarantee an outcome, because I do not know
the codebase yet.

- Wrong: "I'll have the curl examples added by Friday."
- Right: "Next I'll read docs/API.md against the routes under api/ to list which endpoints have no request example, and report what I find here."

### Rule: Name the specifics of this issue

Every claim or report I post names something only this issue has: the
file, the endpoint, the behavior, or the thread pointer. If the
comment would fit another issue unchanged, it is not ready.

- Wrong: "I'd like to work on this issue as my first contribution."
- Right: "I'd like to take #47: docs/API.md documents the endpoints but has no curl example for any of them."

### Rule: Show it, then say it

A statement about behavior comes after the output that shows it. If I
have no output to paste, I write what I did instead of what I
concluded.

- Wrong: "Confirmed, the API docs have no curl examples."
- Right: "`grep -c 'curl' docs/API.md` on commit abc1234 prints 0; the file has 6 endpoint sections and none carries a request example."

### Rule: Say what I did not do

When my check covers less than the issue, I name the gap so a reader
does not credit me with more than I ran.

- Wrong: (say nothing about the endpoints skipped)
- Right: "I did not exercise POST /reviews, which takes a multipart body; this report covers the JSON endpoints only."

### Rule: Plain first person, no filler

I write the way I would speak to a teammate in a code review: no
greetings to "sir", no praise for the project, no adjectives standing
in for evidence.

- Wrong: "Hello sir! Great project, I love it and use it every day. This bug is clearly a race condition."
- Right: "Hi, first contribution here. Repro on commit abc1234 below."

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->

- A date, an ETA, or "guaranteed".
- "Same as above", "can confirm", "+1", or any repro that borrows a classmate's proof.
- A request to reserve or assign the issue to me.
- "Clearly", "obviously", "definitely", or "100%" ahead of the evidence.
- A root cause I did not trace in the code or show in a test.
- An environment I did not record: no report goes up without the commit hash, the OS, and the tool versions I ran.
- A wall of headings and tables where two paragraphs and one code block would do.
