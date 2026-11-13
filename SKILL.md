---
name: olakunlevpn-diataxis-framework
description: Use when writing, reviewing, restructuring, or auditing technical documentation -- tutorials, how-to guides, reference, explanation, READMEs, docs sites, API docs, user guides, onboarding docs. Also use when deciding what kind of doc a piece of content is, where it belongs, why docs feel muddled or hard to navigate, or when someone mentions Diataxis, Diátaxis, "the four quadrants", or documentation architecture. Pass --sudo to require real, working, verified code in every section.
---

# Diátaxis Framework

## Overview

Diátaxis (Greek *dia* "across" + *taxis* "arrangement") is a systematic approach to technical documentation by Daniele Procida (https://diataxis.fr). It identifies **four distinct user needs** and **four corresponding forms of documentation**: tutorials, how-to guides, reference and explanation. Documentation should be organized around the structure of those needs.

**Core principle:** each piece of documentation serves exactly one need. When forms blur together, every form gets worse. Keep them apart, and link between them.

The full site, all 18 pages, is in `references/` as Markdown. This file is the working summary. Read the relevant reference page before doing serious work on one form.

## Parameters

### `--sudo` — Require implementable code

When `--sudo` is passed, every section that describes a feature, behavior, or capability **MUST include real, working code** that implements it.

**Rules:**
1. **No pseudocode.** The code must compile/run as written.
2. **No assumptions.** Don't write "you could do X" or "something like this". Write the actual code.
3. **No placeholders.** No `// TODO`, no `...`, no `<your-code-here>`. Every line must be real.
4. **No hallucinated APIs.** Every function, method, class, or import must exist. Verify it.
5. **Complete enough to work.** A developer or agent copying the code must be able to run it without guessing missing pieces.
6. **Match the documented behavior exactly.** If the doc says "retries 3 times", the code must retry 3 times.

**What this changes per form:**

| Form | Without `--sudo` | With `--sudo` |
|---|---|---|
| **Tutorial** | Steps can reference code conceptually | Every step includes the exact code the learner types, verified to produce the stated output |
| **How-to guide** | Can describe approach without full implementation | Must include complete, working code for the goal |
| **Reference** | Describes API signatures and behavior | Includes working usage examples for every documented item |
| **Explanation** | Discusses concepts and rationale | Any code shown must be real; if discussing implementation, show the actual implementation |

**Failure mode:** If you cannot provide real code (e.g., the feature doesn't exist yet, or you don't have access to verify it), say so explicitly. Do not fabricate code that looks plausible but may not work.

**How to verify:** run the code, or read the library's source or official docs for every call you use. If you did neither, the code is not verified. Say so.

**Example violation** (the doc says "retry failed requests 3 times"):
```python
# ❌ BAD: requests.get has no `retries` argument. This raises TypeError.
import requests

response = requests.get("https://api.example.com/orders", retries=3)
```

**Example compliance:**
```python
# ✅ GOOD: real APIs from requests and urllib3; runs as written
import requests
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

retry = Retry(total=3, backoff_factor=1, status_forcelist=[502, 503, 504])
session = requests.Session()
session.mount("https://", HTTPAdapter(max_retries=retry))

response = session.get("https://api.example.com/orders", timeout=10)
response.raise_for_status()
print(response.json())
```

When `--sudo` is active, treat any code you write as a contract. If a developer follows it and it doesn't work, the documentation has failed.

## The map: two axes, four forms

```
                    ACTION (doing)
                         |
        TUTORIALS        |       HOW-TO GUIDES
   learning-oriented     |     goal/task-oriented
                         |
ACQUISITION -------------+------------- APPLICATION
  (study)                |                (work)
        EXPLANATION      |       REFERENCE
 understanding-oriented  |   information-oriented
                         |
                  COGNITION (thinking)
```

|                  | Tutorials | How-to guides | Reference | Explanation |
|---|---|---|---|---|
| What they do | introduce, educate, lead | guide | state, describe, inform | explain, clarify, discuss |
| Answers the question | "Can you teach me to…?" | "How do I…?" | "What is…?" | "Why…?" |
| Oriented to | learning | goals | information | understanding |
| Purpose | provide a learning experience | help achieve a particular goal | describe the machinery | illuminate a topic |
| Form | a lesson | a series of steps | dry description | discursive explanation |
| Analogy | teaching a child how to cook | a recipe in a cookery book | information on the back of a food packet | an article on culinary social history |

Neighbouring forms share a characteristic, and that is where blur happens:
- Tutorials and how-to guides both describe **practical steps**.
- How-to guides and reference both serve **work**.
- Reference and explanation both contain **theoretical knowledge**.
- Tutorials and explanation both serve **study**.

## The compass: classify any content

Ask two questions: **action or cognition?** and **acquisition or application?**

| If the content… | …and serves the user's… | …then it belongs to… |
|---|---|---|
| informs action | acquisition of skill | a tutorial |
| informs action | application of skill | a how-to guide |
| informs cognition | application of skill | reference |
| informs cognition | acquisition of skill | explanation |

Apply it at any scale: a whole document, a section, a single sentence. Use it whenever you are unsure, or when your intuition gives a quick answer. A quick answer can still be wrong. Read the terms flexibly: action = practical steps, doing. Cognition = theoretical knowledge, thinking. Acquisition = study. Application = work.

## The four forms

### Tutorials: learning-oriented lessons
A tutorial is a **lesson** that takes a learner by the hand through a practical experience. The teacher is responsible for the learner's success. The goal is skill and confidence, not information.
- Show the learner where they'll be going. Deliver visible results early and often.
- Maintain a narrative of the expected ("You'll notice that…", "The output should look like…").
- Point out what the learner should notice. Target *the feeling of doing*. Encourage repetition.
- **Ruthlessly minimise explanation.** Link out to explanation instead. Focus on the concrete.
- Ignore options and alternatives. Aspire to perfect reliability: every step must work, every time.
- Language: "We…", "In this tutorial, we will…", "First, do x. Now, do y.", "Notice that…", "You have built…".

### How-to guides: goal-oriented directions
A how-to guide helps an **already-competent user** get a real-world task done. It is about the user's work and their problem, not the tool's features.
- Address real-world complexity. Users can adapt it to their situation.
- Omit the unnecessary. Practical usability beats completeness.
- Give a set of instructions in a logical sequence. Seek flow, and anticipate the user's next move.
- Don't teach and don't explain. Link to explanation instead.
- Name it precisely: "How to integrate application performance monitoring", not "Integrating APM".
- Language: "This guide shows you how to…", conditional imperatives ("If you want x, do y"), "Refer to the x reference for a full list of options."
- A how-to guide is not a procedure or a list of steps for its own sake. It is about achieving a goal, and it can include branches and judgement.

### Reference: information-oriented description
Reference gives **technical description of the machinery**: APIs, classes, commands, options, config keys. The user consults it. They don't read it.
- **Describe and only describe.** Be neutral, austere, accurate, complete. No instruction, no explanation, no opinion. Link out for those.
- Adopt standard patterns. Consistency lets users find things.
- Respect the structure of the machinery. The docs' structure should mirror the code or product's structure.
- Provide examples to illustrate usage, but keep them succinct.
- Language: state facts ("X is available as…"), list ("Sub-commands are: a, b, c"), warn ("You must use a. Never d.").

### Explanation: understanding-oriented discussion
Explanation **deepens and broadens understanding**. It is read away from the work, in reflection. It answers "why?" and "what about…?"
- Make connections, including connections to things outside the immediate topic.
- Provide context: background, history, design decisions, constraints.
- Talk *about* the subject. Good titles have an implicit "About…" in front of them ("About user authentication").
- Admit opinion and perspective. Weigh alternatives and counter-examples.
- Keep explanation closely bounded. Don't let instruction or reference creep in.
- Language: "The reason for x is because historically y…", "W is better than z because…", "An x in system y is analogous to…", "Some users prefer w (because z)…".

## Tutorials vs how-to guides (most common confusion)

Both are practical steps. The difference is **study vs work**.
- Tutorial: the learner doesn't know what questions to ask yet. The author picks the path, guarantees success, and avoids choices. The learner is *at study*.
- How-to: the user knows what they want and has the competence. The author answers that user's question, and the user carries the responsibility. The user is *at work*.
- Test: "Is the user learning or working right now?" A "Getting started" page that is really a list of setup tasks is a how-to guide. One that teaches by doing is a tutorial.
- Basic vs advanced is *not* the distinction. A how-to guide can be basic, and a tutorial can be advanced.

## Reference vs explanation

Both are theoretical knowledge. The difference is **work vs study**.
- Would someone consult this *while working*, with the product open? That's reference. It should be boring, structured and complete.
- Would someone read this in the bath, or away from the keyboard? That's explanation.
- When reference starts explaining "why", or an explanation starts listing options, move that content out and link to it.

## Workflow: how to apply Diátaxis

Diátaxis is **a guide, not a plan**. Don't start by creating four empty sections and filling them.
1. **Choose something**: whatever is in front of you, such as the file you're in or the last page you read, or something picked at random. Don't go hunting for problems. Keep it small: a page, a paragraph or a sentence.
2. **Assess it**: what user need does it represent? How well does it serve that need? What can be added, moved, removed or changed? Do its language and logic fit this form?
3. **Decide what to do**: pick the single next action that produces an immediate improvement.
4. **Do it**: complete that one action and consider it done. Publish it, or at least commit it.
5. **Repeat.** Structure emerges from the inside as content gets better.

Principles: don't worry about structure. Work one step at a time. Just do something. Don't tear everything down and start again. Let the docs grow organically, like a plant: never finished, but complete at every stage.

## Quality

- **Functional quality**: accurate, complete, consistent, useful, precise. It is objective, measurable and *necessary*. Each quality is independent of the others.
- **Deep quality**: feels good to use, has flow, fits human needs, anticipates the user. It is subjective, interdependent, and *conditional on* functional quality.
- Diátaxis helps expose gaps in functional quality, for example a reference that turns out to be incomplete once separated. It also creates deep quality by aligning form with need. It cannot guarantee accuracy. That is still the author's job.

## Structure: blur, collapse, and the user's journey

- Each form has a natural affinity with its neighbours on the map, so distinctions tend to blur. When they do, style and content leak into the wrong places, and that creates structural problems too.
- Worst case: tutorials and how-to guides collapse into each other, partly or completely. Then neither need is met.
- Users move around the map in a cycle: learning (tutorials), then goals (how-to), then information (reference), then understanding (explanation), and back again. They can enter anywhere, so link between the forms rather than forcing an order.
- Diátaxis changes structure **from the inside**. **Never create empty Tutorials/How-to/Reference/Explanation sections** to fill in later. The site says: "Don't do that. It's horrible." The top-level headings appear when improved content demands them.

## Quick classification and rewrite checklist

When reviewing or writing a doc:
1. Run the compass on the whole doc, then on each section, then on suspicious sentences.
2. Flag content that belongs to another form. Move it, then link to it.
3. Check the language against that form's patterns above.
4. Check the title: tutorials name what you'll build or learn, how-tos start "How to…", reference names the thing, and explanation reads as "About…".
5. Check that the doc serves one user, in one mode (study or work), with one need.

## Common mistakes

| Mistake | Fix |
|---|---|
| Tutorial explains every concept along the way | Cut to one sentence plus a link to explanation |
| Tutorial offers options ("you could also use…") | Pick one path and remove the alternatives |
| How-to guide teaches basics first | Assume competence. Link to a tutorial |
| How-to titled as a feature ("The export command") | Title it by goal: "How to export data to CSV" |
| Reference has opinions or recommendations | Move them to explanation or how-to |
| Reference organized by the docs author's ideas | Mirror the product's structure |
| Explanation buried inside reference or how-to | Pull it out into its own "About…" page |
| Creating four empty sections first, then filling them | Never do this. Improve content one step at a time, and the structure will follow |
| Treating Diátaxis as a rigid template | Use it as a compass and a guide to work |

## Reference files (complete site content)

| File | Content |
|---|---|
| `references/index.md` | Home: overview, contents, testimonials |
| `references/start-here.md` | Primer covering the four forms, with a summary per form |
| `references/application.md` | Applying Diátaxis: index of the practical pages |
| `references/tutorials.md` | Tutorials in full: obligations of the teacher, principles, language, cooking analogy |
| `references/how-to-guides.md` | How-to guides in full: principles, naming, language, recipe analogy |
| `references/reference.md` | Reference in full: principles, language, food-packet analogy |
| `references/explanation.md` | Explanation in full: value, boundaries, language |
| `references/compass.md` | The compass decision tool |
| `references/how-to-use-diataxis.md` | Workflow: Diátaxis as a guide to work, organic growth |
| `references/theory.md` | Understanding Diátaxis: index of the theory pages |
| `references/foundations.md` | Foundations: the two dimensions of craft, why four and only four |
| `references/map.md` | The map: structure, blur, collapse, the journey around the map |
| `references/quality.md` | Theory of quality: functional vs deep quality |
| `references/tutorials-how-to.md` | Tutorials vs how-to guides, in depth |
| `references/reference-explanation.md` | Reference vs explanation, in depth |
| `references/translation.md` | Translating the Diátaxis site |
| `references/colophon.md` | Colophon: author, history, citation, contact |
| `references/news.md` | Site news |

Source: https://diataxis.fr, by Daniele Procida, licensed CC BY-SA 4.0. Extracted 2026-10-15 from the version published 2026-08-06. Diagrams are shown as `*[Diagram: …]*` with their alt text.
