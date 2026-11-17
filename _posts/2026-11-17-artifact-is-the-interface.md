---
layout: post
title: "The Artifact Is the Interface: Output-First Interaction and the Economics of Revision"
subtitle: "A field essay on generative writing tools — design perspective, not empirical claim."
date: 2026-11-17 12:00:00 +0530
categories: [writings, technical-investigations]
tags: [human-ai-interaction, interaction-design, revision]
permalink: /writings/artifact-is-the-interface/
---

*Human–AI interaction · generative interfaces · revision and agency*

## 1. A counterintuitive default

Most generative writing tools still optimize the moment **before** anything exists on the page. The operator imagines an outcome, encodes it as a prompt, and hopes the model’s interpretation matches an intention that has never yet been made concrete.

That workflow is not wrong for every task. It is, however, **structurally expensive** for a class of problems where preferences are *latent*: visible only after reading a real draft. Writers routinely discover that the introduction already works, the voice is right, and a single argument in the middle needs substantive repair—none of which was knowable in advance.

The artifact, once it exists, is not merely an output. It is **information** that reduces uncertainty about what to do next.

Two related ideas are easy to conflate but serve different roles:

- **Output-first interaction** describes the *sequencing* of work: produce an initial artifact before the operator has fully specified every preference, then refine from what is visible rather than from anticipation alone.  
- **Artifact-first interaction** describes the *control surface*: treat the existing artifact—not only the prompt transcript—as the primary place for inspection, selection, modification, preservation, and scoped intervention.

Output-first interaction is about **when** generation happens relative to specification. Artifact-first interaction is about **where** control lives after something exists. A system can be output-first without offering artifact-first controls (e.g., regenerating whole chat turns). Conversely, artifact-first editing can appear in workflows that still front-load heavy prompting. The productive combination is **output-first generation paired with artifact-first revision**—which is the stance this essay develops.

It sits alongside, and partly critiques, prompt-centric design. It also connects to a growing body of HCI work on localized editing, layered writing, and agency in co-creation—work that suggests the next leverage point may be **revision mechanics**, not longer prompts.

---

## 2. From specification burden to evaluation

Pre-generation interaction front-loads **specification**: the operator must predict what the model will misunderstand, which constraints matter, and which dimensions of quality will dominate verification.

Post-generation interaction front-loads **evaluation**: the operator inspects a concrete structure, marks what satisfies, isolates failure modes, and chooses an intervention scoped to the failure. **Artifact-first** control means those evaluations attach to selections, protected regions, and versions of the document—not only to the next message in a thread.

The loop becomes:

1. Generate an initial artifact.  
2. Inspect structure and content.  
3. Protect regions that already satisfy.  
4. Intervene on recognized problems.  
5. Verify that the intervention worked and that nothing else regressed.  
6. Repeat.

This is close to how skilled human editors already work—except that many LLM UIs still behave as if every turn is a full rewrite invitation. Chat threads treat responses as **disposable messages** rather than as **versioned documents** with protected regions and dependency structure.

Output-first sequencing does not eliminate prompting. It relocates effort: less energy spent guessing the full shape of a document in advance; more energy spent on **scoped correction** after partial success is visible, on the artifact as interface.

---

## 3. The economics of revision

A useful objective function is not “maximize human editing” or “maximize model regeneration.” It is **minimize total revision cost** to reach an acceptable artifact.

As an **author-proposed analytical framework**—not a validated scientific metric—revision cost can be summarized as:

**C_revision = C_human + C_inference + C_verification**

| Term | What it captures |
|------|------------------|
| **C_human** | Detecting the problem and communicating the desired change |
| **C_inference** | Model work to regenerate or patch |
| **C_verification** | Reading enough of the artifact to trust that nothing else broke |

This decomposition is a design heuristic. It is not yet a standardized measure across tools or studies, and it should not be reduced to productivity alone: creative exploration can legitimately raise verification time without being “waste.”

Two distinctions sharpen the framework:

**Edit size** is how much text or structure changes in the targeted intervention (a word, a paragraph, a section reorder).

**Dependency footprint** is the set of artifact components *potentially affected* by that intervention—logical dependents, bridging claims, citations, or tone threads—not merely neighbors on the page.

A **large** edit can have a **small** footprint if it is locally isolated (e.g., rephrasing a transition that nothing else presupposes). A **small** edit can have a **large** footprint if it alters a foundational claim that many downstream sentences inherit. Verification cost often tracks **footprint**, not raw edit size.

For a typo, direct human edit usually wins on all three terms. For restructuring an argument, describing the transformation to a model may beat manual rewrite—if verification stays bounded. For a wholesale change in direction, broad model steering may be cheapest *net*, despite a large verification surface.

Failure happens when the **cheapest effective intervention** is not available. A small desired change triggers a global regeneration; verification then dominates, and operators defer fixes or accept defects. That is an interaction-design pathology, not only a capability gap.

**Design implication:** systems should default to the smallest intervention that can plausibly succeed, make verification scope legible, and treat **dependency footprint** as a first-class object—not only the selection’s character count.

---

## 4. An intervention ladder

A practical ladder:

| Rung | When it fits |
|------|----------------|
| **Direct human edit** | Wording, local tone, deletions |
| **Anchored model edit** | Paragraph-level substance, evidence, clarity—selection defines scope |
| **Model-level steering** | Global direction, genre, or style shift |

Artifact-informed choice beats pre-steering everything at once. Inspection reveals which rung applies.

HCI has been exploring the middle rung for several years. **DirectGPT** (CHI 2024) applies direct-manipulation principles to LLM use: operators select regions, localize the effect of commands, and undo at operation granularity ([Masson, Malacria, Casiez, & Vogel, 2024](https://doi.org/10.1145/3613904.3642462)). In a comparative study on text, code, and vector-image editing tasks, participants using DirectGPT were **50% faster**, issued **50% fewer** and **72% shorter** prompts than with baseline ChatGPT, and were **25% more successful** at completing tasks—largely by **localizing** intent to selections rather than re-describing global state in the prompt.

That result supports a broader point: much “prompt engineering” is compensating for missing **selection and scope** on the artifact-first control surface.

---

## 5. Local revision and dependency—not just locality

“Edit paragraph four only” is insufficient if paragraph five bridges to the old version of four. **Local revision** must mean:

- Preserve content that is semantically unaffected.  
- Regenerate targeted units and **dependents**, not everything spatially adjacent.  
- Surface broken dependencies instead of silently rewriting the whole.

**Dependency footprint** names the set of components the system (or operator) treats as potentially impacted by an intervention. Footprint estimation drives **C_verification**: the more components are implicated, the more of the artifact must be re-read or re-checked.

Graphs and linters are one way to approximate footprint—especially for explicit logical relations—but **semantic dependencies are not always graphable cleanly**. Implication, tone, audience stance, and tacit premises may not reduce to reliable edges; any dependency model will be incomplete. Design should treat graphs as **aids to verification**, not as guarantees that the true footprint has been captured.

Recent systems make structural footprint explicit in different ways. **DocuDebug** (UIST 2026) models paragraphs and sources as a **semantic dependency graph** and uses a linter metaphor to flag logical conflicts. Local edits trigger bounded recomputation; when logic breaks, the UI can preserve “broken” edges so the author resolves structure deliberately rather than receiving an opaque global refresh ([Matsuyama & Higuchi, 2026](https://doi.org/10.1145/3830398.3830558)).

In a within-subjects study with twelve professional writers and editors, the authors report a reduction in unnecessary edits to unaffected paragraphs—from **40.8%** (baseline) to **2.8%** (DocuDebug, *p* = .002). Precision increased from **10.3%** to **22.6%**, while diagnostic recall decreased from **63.5%** to **36.5%**; importantly, neither the precision nor the recall difference was statistically significant. These results should therefore not be interpreted as establishing a **statistically demonstrated** trade-off between preservation and detection. They nevertheless raise an important design question: how should interfaces balance unnecessary interventions against the possibility of overlooking problematic content? Operators may still need to exercise independent judgment when a system’s dependency model is incomplete or uncertain.

Parallel lines of work pursue artifact-first structure without identical graph semantics. **Componentization** decomposes monolithic model output into linked, editable units ([Lingo et al., 2025](https://arxiv.org/abs/2509.08203); exploratory study, **n = 4**—treat empirical claims as preliminary). **Synthia** ties feedback items to specific sentences and supports branched, selective regeneration in a feedback-driven workflow ([Zhang et al., 2025](https://doi.org/10.1145/3746059.3747703)). **Textoshop** (CHI 2025) adapts drawing-tool metaphors for moving, reshaping, and layering text ([Masson, Kim, & Chevalier, 2025](https://doi.org/10.1145/3706598.3713862)).

Together, these systems explore structural responses to a shared interaction problem: treating the generative artifact more like **software with inspectable structure** than like a disposable chat bubble—but they do not constitute a single agreed disciplinary solution.

---

## 6. Layered scale: micro-edits and macro-structure

Not every intervention is paragraph-sized. **Script&Shift** (CHI 2025) argues that LLM writing tools can disrupt composition when operators need to interleave **content development** (“scripting”) with **rhetorical reorganization** (“shifting”). Their layered, zoomable workspace supports prompts and operations at multiple scales while aiming to preserve creative control ([Siddiqui, Pea, & Subramonyam, 2025](https://doi.org/10.1145/3706598.3714119)).

Output-first sequencing is compatible with layering; artifact-first control operates at the scale of current focus—sentence, section, or structure—while protection marks define what must not move when the model acts.

The unifying question is not “chat vs. document,” but **which unit of action** matches the operator’s current uncertainty.

---

## 7. Bounded creative autonomy

Full human prescription of every sentence is costly. Unrestricted model freedom destroys accepted regions. Productive co-creation needs a middle distribution:

- **Required constraints** — claims, examples, sections, or relations the operator explicitly cares about.  
- **Delegated creative space** — phrasing, transitions, and detail the operator deliberately leaves open.

The model should innovate inside delegated space without violating constraints. Agency is **regional and dynamic**, not a single dial labeled “human vs. AI.”

Delegation can vary **by region and by rhetorical function** in the same artifact: a factual claim may be fixed; an explanatory paragraph open to substantive revision; a transition open to creative rewording. These boundaries are often clear to the author in the moment but **current systems cannot reliably infer every delegation level** from behavior alone—protection marks, explicit locks, and staged prompts remain necessary.

A 2025 scoping review of 134 HCI/CSCW papers frames co-creation through **agency** and catalogs concrete **control mechanisms**—including modification, intervention, scaffolding, and iterative feedback ([Zhang, Wang, & Yi, 2025](https://doi.org/10.1145/3757594)). Artifact-first revision can be read as emphasizing **post-hoc intervention** and **modification with preserved intent**, rather than front-loaded specification alone.

---

## 8. A compact design principle

Synthesizing the above:

**Generate first. Evaluate second. Intervene precisely. Preserve by default.**

Operational readings:

- Treat artifacts as **durable objects** with versions, selections, and locks (**artifact-first** control).  
- Make blast radius, **dependency footprint**, and incomplete dependency models **visible** when they are not obvious.  
- Prefer the intervention ladder’s lowest successful rung.  
- Use **C_revision** as a heuristic lens—not a creativity score—when comparing interaction designs.

This principle extends beyond prose. The same pattern appears whenever operators must change one region of an image or clip without repainting the entire canvas—selection, constraint, local update, verify.

---

## 9. What this essay does not claim

- It does not assert that output-first or artifact-first interfaces outperform all prompt-first baselines on every task; that requires controlled studies across domains.  
- It does not claim access to model internals; claims here concern **observable artifacts and interaction traces**.  
- It does not replace peer review; cited empirical results belong to their original authors, samples, and conditions.  
- **C_revision**, **dependency footprint**, and the pairing “output-first / artifact-first” are **analytic vocabulary introduced here** unless otherwise cited—they are not established HCI standard terms.

Several recent HCI systems **explore structural responses** to a shared interaction problem: **global regeneration as default edit semantics** raises verification cost and can erode trust in sustained co-authoring. That observation is a pattern in contemporary design research—not a claim that the field has converged on one shared solution.

---

## 10. Open problems

1. **Metrics:** Can workflows be compared using **C_revision** (or successor frameworks) without reducing creative work to throughput?  
2. **Legibility:** How much dependency structure can casual operators absorb without IDE-level complexity?  
3. **Uncertain dependencies:** How should UIs communicate **incomplete footprint models** when graphs under- or over-shoot semantic impact?  
4. **Detection vs. preservation:** How should tools balance unnecessary interventions against overlooked problematic content when empirical precision/recall shifts may not reach significance (as in DocuDebug’s reported descriptive trends)?  
5. **Long sessions:** How do protection marks and edit history compose when context drifts over long human–model threads?  
6. **Inference of delegation:** Can systems learn regional constraint vs. delegation from behavior **without** overfitting or silently mis-locking creative regions?  
7. **Cross-modal unity:** Do text, image, and video tools share one intervention ontology, or only a family resemblance?

These are fertile links for a research archive: concepts such as semantic drift, revision cost, dependency footprint, and agency allocation can be defined, versioned, and connected to future technical notes or prototypes.

---

## 11. Coda: beyond better prompts?

Improvements to one-shot generation will continue. But for knowledge work that **accumulates**—reports, arguments, specifications—the binding constraint is often not the first draft. It is whether the operator can **afford** to keep correcting what is almost right.

If the artifact is the richest signal about what should change, then **artifact-first** control—not only the empty prompt box—may be where the next generation of generative interfaces should compete.

---

## References

- Lingo, R., Chhajer, R., Arroyo, M., Brkljacic, L., Davis, B., & Santhanam, N. (2025). *Componentization: Decomposing Monolithic LLM Responses into Manipulable Semantic Units.* arXiv:2509.08203. https://arxiv.org/abs/2509.08203  
- Masson, D., Malacria, S., Casiez, G., & Vogel, D. (2024). *DirectGPT: A Direct Manipulation Interface to Interact with Large Language Models.* CHI 2024. https://doi.org/10.1145/3613904.3642462  
- Masson, D., Kim, Y.-H., & Chevalier, F. (2025). *Textoshop: Interactions Inspired by Drawing Software to Facilitate Text Editing.* CHI 2025. https://doi.org/10.1145/3706598.3713862  
- Matsuyama, H., & Higuchi, K. (2026). *DocuDebug: A Graph-Supported Interactive Editor for Verifying and Maintaining Internal Logical Consistency.* UIST 2026. https://doi.org/10.1145/3830398.3830558  
- Siddiqui, M. N., Pea, R. D., & Subramonyam, H. (2025). *Script&Shift: A Layered Interface Paradigm for Integrating Content Development and Rhetorical Strategy with LLM Writing Assistants.* CHI 2025. https://doi.org/10.1145/3706598.3714119  
- Zhang, C., Ju, K., Han, Z., Yen, Y.-C. G., & Rzeszotarski, J. M. (2025). *Synthia: Visually Interpreting and Synthesizing Feedback for Writing Revision.* UIST 2025. https://doi.org/10.1145/3746059.3747703  
- Zhang, S., Wang, H., & Yi, X. (2025). *Exploring Collaboration Patterns and Strategies in Human-AI Co-creation through the Lens of Agency: A Scoping Review of the Top-tier HCI Literature.* PACMHCI (CSCW), 9(7), Article CSCW413. https://doi.org/10.1145/3757594  

*Design essay. Terms in §9 are author-proposed unless otherwise cited.*
