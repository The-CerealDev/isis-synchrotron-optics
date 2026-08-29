# ISIS Work Experience — CV & LinkedIn copy

Drafted from the actual commit history of the placement repo (20–23 July 2026).
Every claim below maps to real work. Notes on what to be ready to defend are at the bottom.

---

## 1. CV entry (main version)

**Work Experience Placement — Accelerator Controls / EPICS Archiver Team**
STFC ISIS Neutron and Muon Source, Rutherford Appleton Laboratory — July 2026 (1 week)

- Wrote Python modules for the backend of an accelerator optics GUI, retrieving live machine
  data (corrector currents, trim-quadrupole settings, requested and harmonic tunes) from the
  facility's EPICS archiver REST API.
- Diagnosed and fixed a ~5-second-per-keystroke lag in a search tool covering 40,000+ process
  variables, by vectorising the parsing with pandas, caching repeated normalisation and
  debouncing input — reduced to near-instant response.
- Tested the team's internal API documentation against the live archiver, found a documented
  endpoint behaviour that did not actually work, and replaced it with a method that did.
- Investigated a set of harmonic-tune signals missing from the archiver; established through
  exhaustive search that it was a gap in the test rig rather than a naming error, and recorded
  it as an open question for staff instead of coding around it.
- Built an interactive 3D model of the synchrotron in JavaScript/WebGL, generating the ring
  layout from real MAD-X lattice dipole bend angles rather than an idealised circle.

**Tools:** Python (pandas, NumPy, Streamlit), REST APIs, JavaScript/WebGL, Git, EPICS, MAD-X

---

## 2. CV entry (short version — if space is tight)

**Work Experience, Accelerator Controls** — STFC ISIS Neutron and Muon Source, July 2026

One-week placement with the EPICS archiver team on the backend of an accelerator optics GUI.
Wrote Python to pull live magnet and tune data from the facility's archiver REST API; fixed a
performance problem in a 40,000-record search tool that had made it unusable; corrected an
error in the team's API documentation after testing it against the live service.
*Python, pandas, REST APIs, JavaScript/WebGL, Git.*

---

## 3. LinkedIn — Experience section

**Title:** Work Experience Placement, Accelerator Controls
**Company:** STFC ISIS Neutron and Muon Source
**Dates:** July 2026 · 1 week
**Location:** Rutherford Appleton Laboratory, Didcot (On-site)

**Description:**

A week with the EPICS archiver team, working on the backend for a GUI that models the optics of
the ISIS Rapid Cycling Synchrotron.

What I worked on:

• Python modules that read live machine data — corrector currents, trim-quadrupole settings,
  requested and harmonic tunes — from the facility's EPICS archiver REST API.
• A performance fix for a process-variable search tool: it covered 40,000+ PVs and lagged about
  five seconds on every keystroke. Vectorising the parsing and caching the repeated work made it
  usable.
• Verifying the team's API notes against the live archiver. One documented behaviour turned out
  not to work as written, so I corrected the documentation and added a method that did work.
• Interactive 3D model of synchrotron in JavaScript / Three.js, with ring geometry built from real MAD-X lattice dipole bend angles.

The most useful thing I learned was how much of the work is verification — checking that what the
documentation says actually matches what the machine does, and being willing to report "this data
genuinely isn't there" rather than inventing a value to make the code run.

**Skills to tag:** Python · pandas · REST APIs · JavaScript · Git · Data Visualisation ·
Problem Solving · Technical Documentation

---

## 4. LinkedIn — announcement post (optional)

> Just finished a week of work experience with the accelerator controls group at the STFC ISIS
> Neutron and Muon Source.
>
> I was with the EPICS archiver team, writing Python for the backend of a GUI that models the
> optics of the ISIS synchrotron — mostly pulling live magnet and tune data out of the facility's
> archiver API.
>
> Two things stuck with me. First, a search tool over 40,000+ process variables was lagging about
> five seconds on every keystroke; rewriting the parsing to be vectorised rather than row-by-row
> fixed it, and that was a very concrete lesson in why the shape of your code matters at scale.
>
> Second, and less expected: I spent a good chunk of the placement checking that the documentation
> matched reality. One documented API behaviour didn't actually work. Some data we expected to
> exist simply wasn't in the archiver. The right answer in both cases was to report it accurately,
> not to paper over it — which is not the instinct you develop from coursework.
>
> Thanks to everyone on the team who answered my questions.
>
> #WorkExperience #STFC #ISIS #Python #Accelerators

---

## Notes — read before you post

**Be ready to explain these in an interview.** Anything on a CV is fair game for "talk me through
that." The two you're most likely to be asked about:

1. **The performance fix.** Know why vectorised pandas operations beat a Python loop over rows,
   and roughly what caching and debouncing each solved. This is the strongest bullet you have —
   it's a measurable before/after.
2. **The missing harmonic data.** The point of this one isn't the searching, it's the judgement:
   you concluded the data was absent and flagged it, rather than faking a value. Interviewers
   like that story. Be able to say what you searched and why you were confident.

**On AI assistance.** This work was done with AI tooling. That's completely normal and not
something to hide, but do make sure you can speak to *why* each change was made, not just what it
did — that's what the claims above rest on. If you can't explain a bullet, cut it.

**Two things I deliberately left out**, because they're weaker than they sound: the BPM readback
investigation (real work, but it was parked without being used) and the Tune group code review
(hard to evidence, and it involves judging named colleagues' work — not something to put in
public). Mention them verbally if relevant, not in writing.

**Check before publishing:** whether STFC is happy for you to name internal systems publicly. The
drafts above avoid hostnames and internal URLs, but the LinkedIn post is a public statement about
a national facility's controls infrastructure — worth a quick ask to your supervisor.
