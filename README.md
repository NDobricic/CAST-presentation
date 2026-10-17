# CAST presentation

Collaborative slides for **Programming Refusal with Conditional Activation
Steering** (Lee et al., ICLR 2025).

- [`presentation.tex`](presentation.tex): the single presentation source, including
  styling, editable TikZ charts, references, and speaker notes. Contains the title
  slide, six experimental-analysis slides, and one clearly labeled backup slide.
- [`presentation.pdf`](presentation.pdf): the compiled slides, updated automatically on push.
- [`paper.pdf`](paper.pdf): the official conference paper, including appendices.
- [`AGENTS.md`](AGENTS.md): guidance for contributors and coding agents.

## Upload to Prism

In [Prism](https://prism.openai.com/), choose **Import**, then **Import a .zip file**,
and select `cast-presentation-prism.zip`. The archive places `presentation.tex`
at its root as the only TeX source, alongside the reference `paper.pdf`,
`AGENTS.md`, this README, and a compiled snapshot named `preview.pdf`.
`preview.pdf` is for comparison; compile `presentation.tex` to produce the slides.
If prompted for the main document, select `presentation.tex` and use pdfLaTeX.
No local LaTeX installation is required for collaborators using Prism.

Start the assistant with:

> Read and follow the PRESENTATION EDITING GUIDELINES at the beginning of
> presentation.tex before making changes. Keep those comments in the source.
> After building, visually inspect every slide; if you cannot inspect the output,
> explicitly ask a collaborator to review it.

The editing rules are embedded as LaTeX comments because automatic `AGENTS.md`
loading in Prism is not documented. The comments do not appear in the slides.
Invite collaborators using their Prism accounts. Export the project when ready
to bring source changes back to GitHub; the documented Prism workflow currently
does not include Git synchronization.
See [OpenAI's Prism import/export guide](https://help.openai.com/en/articles/20001050-troubleshooting-and-getting-help-in-prism).

To regenerate the upload archive after building and visually reviewing the PDF,
run this from the repository root (Python 3 is only needed for packaging):

```sh
python3 - <<'PY'
from pathlib import Path
from zipfile import ZipFile, ZIP_DEFLATED

Path("build").mkdir(exist_ok=True)
with ZipFile("build/cast-presentation-prism.zip", "w", ZIP_DEFLATED) as archive:
    for name in ("presentation.tex", "paper.pdf", "AGENTS.md", "README.md"):
        archive.write(name, name)
    archive.write("presentation.pdf", "preview.pdf")
PY
```

## Set up LaTeX

Install a TeX distribution with **pdfLaTeX**, **Beamer**, **PGF/TikZ**, and Latin
Modern fonts. `latexmk` is convenient but optional. No Python, model weights, or
experiment environment is needed.

| Platform | Setup resources |
| --- | --- |
| Linux | [TeX Live](https://tug.org/texlive/) and its [installation guide](https://tug.org/texlive/quickinstall.html) |
| macOS | [MacTeX](https://tug.org/mactex/) |
| Windows | [MiKTeX installation](https://miktex.org/howto/install-miktex), or [TeX Live](https://tug.org/texlive/) |
| Browser | Upload `presentation.tex` to [Overleaf](https://www.overleaf.com/) and choose pdfLaTeX |

On Debian/Ubuntu, the relevant packages can be installed with:

```sh
sudo apt update
sudo apt install texlive-latex-recommended texlive-latex-extra texlive-pictures lmodern latexmk
```

Restart your terminal after installation if necessary, then check `pdflatex --version`.
See the [LaTeX Project setup overview](https://www.latex-project.org/get/) for other systems.

## Automatic builds on GitHub

Every push to a branch runs [Build presentation PDF](.github/workflows/build-presentation.yml).
The workflow installs LaTeX, compiles twice, renders every slide, and commits only
`presentation.pdf` back to the same branch if its bytes changed. It also supports
manual runs from the repository's Actions tab. No local LaTeX installation or
personal access token is needed for this workflow.

Open the [Actions tab](https://github.com/NDobricic/CAST-presentation/actions), select
the run, and download **presentation-preview** for the PDF, PNG images of every
slide, and the compilation log. Artifacts are retained for seven days.
**Visually inspect every slide after each build**; a successful run does not
verify visual quality. Automatic builds commit before this human review.

The workflow uses the built-in `GITHUB_TOKEN` with `contents: write`; commits made
with that token do not trigger another push workflow. If a branch advances during
the build, the workflow skips its obsolete PDF and lets the newer push build it.
It never force-pushes. Pull before your next local edit to receive the bot's commit.

Repository or organization policies must permit workflow write access and the
bot's direct push to the branch. If a branch requires pull requests, its protection
can reject the automatic commit; the build artifact remains available.
See [GitHub token permissions](https://docs.github.com/en/actions/tutorials/authenticate-with-github_token)
and [workflow-trigger behavior](https://docs.github.com/en/actions/how-tos/write-workflows/choose-when-workflows-run/trigger-a-workflow).

## Build the slides

From this repository's root:

```sh
mkdir -p build
pdflatex -interaction=nonstopmode -halt-on-error -file-line-error -output-directory=build presentation.tex
pdflatex -interaction=nonstopmode -halt-on-error -file-line-error -output-directory=build presentation.tex
```

Run twice to resolve slide numbers and references. The output is
**`build/presentation.pdf`**. Alternatively, `latexmk` runs the necessary passes:

```sh
latexmk -pdf -interaction=nonstopmode -halt-on-error -outdir=build presentation.tex
```

On Windows, create `build` in your file manager or with `mkdir build` first,
then run the same pdfLaTeX commands in PowerShell.

## Speaker notes

The six-slide experimental section takes approximately **6 minutes 30 seconds**;
the optional backup is additional. Every slide has embedded Beamer `\note{...}`
content covering interpretation, methods, caveats, provenance, and timing. Notes
are hidden in the audience PDF. Export a separate notes PDF from the same source:

```sh
pdflatex -interaction=nonstopmode -halt-on-error -file-line-error -output-directory=build -jobname=presentation-notes '\def\presenternotes{1}\input{presentation.tex}'
pdflatex -interaction=nonstopmode -halt-on-error -file-line-error -output-directory=build -jobname=presentation-notes '\def\presenternotes{1}\input{presentation.tex}'
pdftoppm -png -r 120 build/presentation-notes.pdf build/notes
```

Visually inspect every notes page too. The output is
`build/presentation-notes.pdf`; all notes and chart data remain in the single
`presentation.tex` source. The experimental values follow the parent project's
completed `notes/ablation-experiments-overview.md` (2026-10-16); they are our
independent ablations, not results reported by the paper.

## Review and collaborate

After **every build**, open `build/presentation.pdf` and visually inspect **all
slides** for clipping, overlaps, legibility, spacing, and consistent styling.
For image previews, install [Poppler](https://poppler.freedesktop.org/) and run:

```sh
pdftoppm -png -r 120 build/presentation.pdf build/slide
```

Inspect every generated `build/slide-*.png`; a successful compiler exit alone
does not establish that the slides look correct. Fix issues, rebuild, and inspect again.
Keep the deck concise and add citations alongside paper claims and figures.
After visual review, update the tracked PDF from the build output:

```sh
cp build/presentation.pdf presentation.pdf
```

In PowerShell, use `Copy-Item build/presentation.pdf presentation.pdf`.
Commit `presentation.pdf` together with source changes and discuss them through
GitHub pull requests. Build outputs, previews, and compilation files under
`build/` remain ignored.

Useful references: [Beamer documentation](https://ctan.org/pkg/beamer),
[TikZ/PGF documentation](https://ctan.org/pkg/pgf), and the
[authors' code](https://github.com/IBM/activation-steering).

## Paper provenance

Bruce W. Lee, Inkit Padhi, Karthikeyan Natesan Ramamurthy, Erik Miehling,
Pierre Dognin, Manish Nagireddy, and Amit Dhurandhar.
*Programming Refusal with Conditional Activation Steering*. ICLR 2025.

Downloaded from the [official ICLR proceedings](https://proceedings.iclr.cc/paper_files/paper/2025/file/e2dd53601de57c773343a7cdf09fae1c-Paper-Conference.pdf)
on 2026-10-15. The paper remains the authors' work; inclusion does not relicense it.

SHA-256 (`paper.pdf`):
`0753abfafcdee98dd230e2c0c207151c8ccdd947ac0ba5d235f8b574f34f1501`.

Verify with `sha256sum paper.pdf` (Linux), `shasum -a 256 paper.pdf` (macOS),
or `Get-FileHash paper.pdf -Algorithm SHA256` (PowerShell).
