# CAST presentation

Collaborative slides for **Programming Refusal with Conditional Activation
Steering** (Lee et al., ICLR 2025).

- [`presentation.tex`](presentation.tex): the single presentation source, including
  styling, diagrams, and references. Contains a title slide and one diagram/style example.
- [`presentation.pdf`](presentation.pdf): the compiled slides, committed after visual review.
- [`paper.pdf`](paper.pdf): the official conference paper, including appendices.
- [`AGENTS.md`](AGENTS.md): guidance for contributors and coding agents.

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
