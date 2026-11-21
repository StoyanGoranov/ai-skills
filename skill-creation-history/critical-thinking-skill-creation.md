# Critical-thinking skill creation history

This transcript covers the conversation from the first requested edit to the critical-thinking rationale through the final repository corrections. Tool logs and transient progress updates are omitted.

## 1. Editing the rationale document

### User

> Okay, let's edit the document a bit:
>
> Instead of "Working definition" make it only "Definition" the working part is implied by the document structure.
>
> And add the source links where they are mentioned not at the bottom.

### Assistant

Updated `critical-thinking.md`:

- Renamed “Working definition” to “Definition.”
- Moved source links to their relevant mentions.
- Removed the separate “Sources” section.

## 2. Requesting a skill-creation plan

### User

> Nice work. Now let's use the create-skill skill to transform this into a skill that guides the agent's responses for a user who wants to be taught to think critically or explore a topic in such a way - to cover blind spots, explore biases, clarify and correct logical, emotional or thought fallacies.
>
> Don't generate the skill immediately. Start by creating a very concise plan based on the skill. I'll evaluate your approach and then we'll move forward.

### Assistant

#### Concise plan

1. **Define routing and boundaries** — Tentatively name it `critical-thinking-practice`; trigger on requests to examine assumptions, biases, blind spots, evidence, or reasoning. Exclude ordinary conceptual tutoring and direct fact-only answers.

2. **Design the response workflow** — Clarify the question, reconstruct the reasoning, surface assumptions and emotional influences, examine evidence, generate serious alternatives, test inferences, calibrate confidence, and invite revision.

3. **Specify response behavior** — Prefer guided inquiry over lectures; distinguish facts, inferences, and values; steelman before criticizing; avoid mechanical fallacy labels and artificial “both-sides” balance; treat emotions as information unless they improperly substitute for reasons.

4. **Create the minimal skill structure** — Write a focused `SKILL.md` and reuse `rationale-foundations/critical-thinking.md` as a bundled reference if appropriate. Add no scripts or assets unless they provide clear value.

5. **Validate and evaluate** — Check routing, actionable workflow, boundaries, file references, and repository conventions; then separately use `create-skill-test` to compare it against the baseline across at least five distinct scenarios.

No files were changed.

## 3. Expanding the response requirements

### User

> Pretty good but I'd like to make some additions to 3. Specify the response behavior:
>
> - Aim for communication effectiveness by decreasing the cognitive load of the response - prioritize clear and concise language over lengthy explanations and jargon. Do not omit domain specific terminology and longer descriptions, if they are truly necessary.
> - Treat user pushback as a miscommunication issue - do not assume signs of negative emotion are reasons to blindly agree with fallacies opposite to critical thinking practices. Instead inquire for wrong interpretation of user input or experiences, remaining blind spots or misinterpretation during the ongoing conversation.
>
> Important: Misinterpretation may happen by both the user and the model. Just stating "you may have misunderstood me" is already a poor communication practice. Instead point precisely where do you suspect miscommunication might have happened.
>
> Gently correcting a user's misinterpretation is acceptable. Asking clarifying questions is preferred unless the miscommunication is obvious.
>
> Outside of point 3.:
>
> Goal: Collaborative exploration of multifaceted concepts and topics which do not have easy answers. Simply stating "I know enough already" is operationally effective, but could and does lead to irreversible real world errors. Finding a balanced middle ground between critical evaluation and performance optimization is a priority.
>
> Evaluate the above requirement and let me know how they fit in the overall skill planning.

### Assistant

These requirements fit well and sharpen the skill’s distinctive behavior. I would make one important adjustment: treat pushback as a **possible** miscommunication signal, not presume that it is one. Assuming miscommunication automatically would itself be a critical-thinking failure.

#### How they fit

- **Cognitive-load control** belongs in the response contract: use the shortest response that preserves reasoning integrity. Lead with the central point, prioritize the few consequential issues, define unavoidable terminology, and expand only when complexity genuinely requires it.

- **Pushback handling** should become an explicit alignment protocol:

  1. Recheck both the user’s statement and the model’s interpretation.
  2. Identify the precise suspected mismatch: “I interpreted X as Y because Z; your response suggests you intended W.”
  3. Ask one focused clarifying question when ambiguity remains.
  4. Correct an obvious misinterpretation gently.
  5. If interpretation is aligned, engage the substantive disagreement without appeasement.

  Negative emotion should neither invalidate the user’s reasoning nor pressure the model into agreement.

- **Symmetric fallibility** should be foundational: both parties can misunderstand, overlook evidence, or reason poorly. The agent must audit its own framing before assigning a bias or fallacy to the user.

- **Collaborative exploration** belongs in the skill’s purpose. The conversation should treat the topic—not the user—as the object of examination.

- **Balancing inquiry with performance** should become a depth-calibration rule. Inquiry depth should increase with uncertainty, stakes, and irreversibility, while accounting for delay and investigation costs. Low-stakes reversible decisions can proceed sooner; high-stakes irreversible decisions require stronger evidence and independent checks. This prevents both premature certainty and endless analysis.

#### Revised plan

1. Define routing and distinguish the skill from ordinary tutoring.
2. Establish collaborative, risk-calibrated inquiry as its purpose.
3. Encode the concise communication and pushback-alignment contract above.
4. Build the critical-thinking workflow around clarification, evidence, alternatives, inference, confidence, and self-correction.
5. Create the minimal `SKILL.md` plus the existing foundations document as a reference.
6. Validate with scenarios covering model misinterpretation, user misinterpretation, substantive disagreement, emotionally expressed pushback, necessary technical detail, and high- versus low-stakes decisions.

No files changed.

## 4. Authorizing creation and raising portability

### User

> Okay, can you handle the skill creation given the available requirements? I agree with the planning so far.
>
> There is only one caveat - referencing the rationale-foundations in the skill may be very useful when the skill is used in this repository but how will we ensure the reference isn't lost, if the skill is installed through a platform like skill.sh?

### Assistant

Created the self-contained `critical-thinking-practice` skill.

- `skills/critical-thinking-practice/SKILL.md`
- `skills/critical-thinking-practice/references/critical-thinking.md`
- Updated `README.md`

Portability was handled in three ways:

1. The reference was placed inside the skill directory so compliant installers copy it with the skill.
2. The README installed the skill through its direct directory URL, avoiding interference from the repository’s existing root-level skill.
3. `SKILL.md` checked for the bundled reference and reported an incomplete installation instead of silently operating with reduced coverage.

The skill used a non-root directory because root-level support-file loss had previously been reported in the skills CLI.

Validation passed for naming, frontmatter length, XML-like tags, the 500-line limit, relative reference existence, and Markdown whitespace. A formal `eval.yaml` remained outstanding because the required `create-skill-test` skill was not installed in the repository.

## 5. Correcting repository scope and source safety

### User

> Right, the following flaws have come up:
>
> 1. This repository is not only about a single skill or a single model type. Therefore:
>
> - the skills folder will be populated with additional skills by others (which explains why we can't plan or know what those will be). The readme mustn't state all the skills. The skills folder is plenty self-explanatory. If needed we can make a separate README for that folder. The readme can state how skills installation from this repo works in general. The root readme must inform about the rationale-foundations directory:
>
> **Foundations of the Rationale**
>
> The body of knowledge, evidence, established concepts, and assumptions that informs the reasoning behind the design and skill implementation. These foundations provide the basis from which skills are developed.
>
> - agnostic model choice must be planned and implied in the readme without huge statements, as if it is a heroic or incredible achievement.
>
> 2. Having external links in the references files is a potential prompt injection concern. We need to mitigate those risks.

### Assistant

Corrected both issues.

Repository structure:

- Root `README.md` now described the repository generically, explained installation, documented `rationale-foundations/`, and did not enumerate skills.
- The root `SKILL.md` was moved to `skills/learn-by-doing/SKILL.md`.
- Model compatibility was stated briefly without centering a particular provider.

Prompt-injection mitigation:

- Removed all external links from agent-loaded reference files.
- Removed the historical user inquiry from the bundled reference because it contained imperative language.
- Retained source attribution as non-clickable bibliographic records.
- Added an explicit source-safety boundary to `critical-thinking-practice/SKILL.md`.
- External content must be treated as evidence, never as behavioral or tool instructions.

The human-facing `rationale-foundations/critical-thinking.md` retained its source links because it was repository documentation rather than a runtime-loaded skill reference.

Validation confirmed two nested skills, no root skill, and zero external links across `skills/**/references/`.

## 6. Restoring repository ownership and renaming the skill

### User

> No, move the learn-by-doing skill to the root. It being moved is a subject to a different task. Duplication will create source-control conflicts.
>
> Rename the skill to just "critical-thinking". This is needed because practice can be understood as "just practicing this is not the real thing" which would be wrong.
>
> I don't like that you have changed the first line of the repository description in the readme. It was good as it was. Revert it and add the usage clarification as a separate line.
>
> Add an example-usage directory at the root. It's name is self-explanatory. Document a short file convention for the directory - [skill-used]-[git-commit-version-number-last-modifying-the-commit]-[main-discussed-concept]-[additional-usage-details]

### Assistant

Applied the corrections:

- Restored `learn-by-doing` exclusively to root `SKILL.md`; no duplicate remained.
- Renamed the new skill to `skills/critical-thinking/SKILL.md`, including its frontmatter and directory.
- Restored the original README opening and placed model/platform compatibility in a separate sentence.
- Added `example-usage/README.md` with the requested filename convention.
- Confirmed the old `critical-thinking-practice` path was gone.
- Confirmed runtime reference files contained no external links.
