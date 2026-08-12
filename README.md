# Resume

## Compile to PDF

### Option A: Docker / Colima (recommended, no local LaTeX install needed)

This repo's `.tex` uses packages (`fullpage`, `fontawesome5`, `titlesec`, etc.)
that aren't included in a minimal/BasicTeX install, so a full TeX Live image
is the most reliable way to compile without fighting missing packages.

If you don't have Docker Desktop (or don't want its org sign-in requirement),
use [Colima](https://github.com/abiosoft/colima) instead:

```bash
brew install colima docker   # one-time setup
colima start                 # starts a local docker daemon, no login required
```

Then compile (run twice so cross-references/outlines resolve):

```bash
docker run --rm -v "$PWD:/data" -w /data texlive/texlive:latest \
  pdflatex -interaction=nonstopmode resume.tex

docker run --rm -v "$PWD:/data" -w /data texlive/texlive:latest \
  pdflatex -interaction=nonstopmode resume.tex
```

This will generate the `resume.pdf` file.

### Option B: Native pdflatex (MacTeX / BasicTeX)

If you have a full LaTeX distribution installed (e.g. MacTeX/BasicTeX), make
sure its binaries are on your `PATH`, then run:

```bash
export PATH="/Library/TeX/texbin:$PATH"
pdflatex -interaction=nonstopmode resume.tex
pdflatex -interaction=nonstopmode resume.tex
```

If you get `! LaTeX Error: File 'fullpage.sty' not found.` (common with
BasicTeX, which only ships a minimal package set), install the missing
packages with:

```bash
sudo tlmgr update --self
sudo tlmgr install collection-latexextra
```

This is a large download and can take a while, so Option A is usually faster
if you just need a one-off compile.
