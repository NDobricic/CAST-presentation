# Goal

Collaborate on a presentation of *Programming Refusal with Conditional Activation
Steering* (ICLR 2025). Use `paper.pdf` as the primary reference.

# Presentation

- Keep all presentation source, styling, diagrams, and references in the single
  `presentation.tex` file. Avoid additional TeX files or a separate bibliography.
- Keep the slide-editing guidance in the leading comments of `presentation.tex`
  consistent with these rules so it travels with browser-editor imports.
- Keep slides clean and concise: one main idea per slide, short text, readable
  type, and generous whitespace. Prefer useful diagrams to dense bullet lists.
- Maintain a visually coherent layout, typography, spacing, and restrained color
  palette throughout the deck.
- Use colors in diagrams to clarify roles, stages, and relationships. Apply the
  same colors consistently and retain clear labels so color is not the only cue.
- Never mention local files, filenames, or paths on slides, including PDFs and
  local figure assets. Cite the paper or original source instead.
- Never describe the presentation's current state on slides: omit labels such as
  "draft", "work in progress", completion notes, and editing instructions.
- Cite paper claims and figures. Clearly distinguish the authors' findings from
  our observations, hypotheses, or reproduction results; do not invent results.
- For interactive work, after every PDF build, visually inspect every slide in a PDF viewer or
  rendered page images. Check clipping, overlaps, legibility, spacing, equations,
  and visual consistency; fix issues and rebuild before considering work done.
- The user authorized GitHub Actions to rebuild and commit the PDF on every push.
  Automated builds may commit immediately after compiling and rendering all slides.
  Publish the PDF and slide previews for contributors to visually review every
  build; never claim that CI compilation or rendering performs visual inspection.
- Follow `README.md` for building. Keep auxiliary files and previews in the
  ignored `build/` directory. After visual review, copy the built PDF to the
  tracked `presentation.pdf` and commit it with source changes. Retain the
  reference `paper.pdf` in Git.
- Commit completed changes in logical groups. Use single-line commit messages
  with no body or coauthor trailers.
