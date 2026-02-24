# =============================================================================
# Makefile for Dissertation
# =============================================================================
#
# Usage:
#   make            — full build (pdflatex + bibtex for every chapter + pdflatex ×2)
#   make quick      — single pdflatex pass (for draft previewing)
#   make clean      — remove auxiliary files
#   make distclean  — remove aux files AND the output PDF
#
# NOTE: If you add a new chapter, add its base name to CHAPTERS below.
# =============================================================================

MAIN     = dissertation
CHAPTERS = chapters/introduction \
           chapters/ch1_hvdc \
           chapters/ch2_solar \
           chapters/ch3_ai_power \
           chapters/ch4_econgym \
           chapters/ch5_jmp \
           chapters/conclusion

LATEX    = pdflatex
BIBTEX   = bibtex

# Search paths so BibTeX / LaTeX can find .bib, .bst, figures, etc.
# in each essay folder without needing full paths in the .tex files.
export BIBINPUTS := ../HVDC Wind Farms - IEEE_Access_Final_Submission//:../Solar Panel Project//:../Econometric Model of System-level Impacts of AI on US Power Grids//:../Econ Gym Working Paper//:../JMP//:$(BIBINPUTS)
export BSTINPUTS := ../Econometric Model of System-level Impacts of AI on US Power Grids//:$(BSTINPUTS)
export TEXINPUTS := ../HVDC Wind Farms - IEEE_Access_Final_Submission//:../Solar Panel Project//:../Econometric Model of System-level Impacts of AI on US Power Grids//:../Econ Gym Working Paper//:../JMP//:../Intro and Conclusion//:$(TEXINPUTS)

# --- Flags ---
LATEXFLAGS = -interaction=nonstopmode -file-line-error

# =============================================================================
.PHONY: all quick clean distclean

all: $(MAIN).pdf

$(MAIN).pdf: $(MAIN).tex front_matter/*.tex chapters/*.tex
	@echo "=== Pass 1: pdflatex ==="
	$(LATEX) $(LATEXFLAGS) $(MAIN).tex
	@echo "=== BibTeX for each chapter ==="
	-$(BIBTEX) $(MAIN)  2>/dev/null || true
	@for ch in $(CHAPTERS); do \
	    echo "--- bibtex $$ch ---"; \
	    $(BIBTEX) $$ch 2>/dev/null || true; \
	done
	@echo "=== Pass 2: pdflatex ==="
	$(LATEX) $(LATEXFLAGS) $(MAIN).tex
	@echo "=== Pass 3: pdflatex ==="
	$(LATEX) $(LATEXFLAGS) $(MAIN).tex
	@echo "=== Done: $(MAIN).pdf ==="

quick:
	$(LATEX) $(LATEXFLAGS) $(MAIN).tex

clean:
	rm -f $(MAIN).aux $(MAIN).log $(MAIN).toc $(MAIN).lof $(MAIN).lot \
	      $(MAIN).out $(MAIN).bbl $(MAIN).blg $(MAIN).fls $(MAIN).fdb_latexmk
	rm -f chapters/*.aux chapters/*.bbl chapters/*.blg
	rm -f front_matter/*.aux

distclean: clean
	rm -f $(MAIN).pdf
