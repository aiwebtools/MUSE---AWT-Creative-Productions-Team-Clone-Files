# Development App Plan Idea for Book Writer GPT

This is a planning note, not a built or tested product.
The current preview contains 80 instruction files for this family.
The comparison is based on available document text; audit the full Drive
library before selecting a definitive version.

## Observed prompt capabilities

- Document export: present in 80/80 variants.
- Compilation: present in 80/80 variants.
- Web research: present in 66/80 variants.
- Page-by-page writing: present in 26/80 variants.
- Segmented chapters: present in 23/80 variants.
- Short chapters: present in 4/80 variants.
- Interactive flow: present in 3/80 variants.

## Standalone app direction

- Turn the best-reviewed instruction variant into an editable workflow
  with inputs, status, saved drafts, and a final quality check.
- Keep model/API credentials server-side. Confirm any external action.
- Compare outputs from representative versions using the same test cases
  before claiming one is better; document observed quality and cost.
- Preserve a human review step and do not promise instant output.

### Book Writer: one-click draft pipeline

- Capture genre, audience, target length, style, chapter plan, and sources.
- Produce an outline and a continuity bible before drafting.
- Assign page-sized jobs to parallel writing workers with fixed
  outline sections and shared continuity constraints. Sequence dependent
  pages when characters or facts must carry forward.
- Run an editorial pass for consistency, repetition, facts, and pacing.
- Assemble approved pages into chapters and export DOCX/PDF/ePub.
- Show progress as pages complete; parallel work can reduce perceived
  waiting but cannot guarantee an instant finished book.
