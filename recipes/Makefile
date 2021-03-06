alectryon := ../alectryon.py --output-directory output --no-header
minimal := python3 -m alectryon.minimal

assets := alectryon.css alectryon.js
all := plain.v.html literate.v.html coqdoc.v.html coqdoc.html literate.html document.html hyps.html document.minimal.html document.v literate.v.rst fragments.io.json fragments.snippets.html fragments.snippets.tex latex.snippets.tex api.out

all: $(patsubst %,output/%,$(all) $(assets)) sphinx/_build/html/index.html;

output:
	mkdir $@

# Coq → HTML
output/plain.v.html: plain.v | output
	$(alectryon) --frontend coq $<
output/literate.v.html: literate.v | output
	$(alectryon) --frontend coq $<
output/coqdoc.v.html: coqdoc.v | output
	$(alectryon) --frontend coq $<

# Coqdoc → HTML
output/coqdoc.html: coqdoc.v | output
	$(alectryon) $< --frontend coqdoc

# Coq+reST → HTML
output/literate.html: literate.v | output
	$(alectryon) $<

# reST+Coq → HTML
output/document.html: document.v.rst | output
	$(alectryon) $<
output/hyps.html: hyps.rst | output
	$(alectryon) $<

# Minimal reST+Coq → HTML
output/document.minimal.html: document.v.rst | output
	cd ..; $(minimal) recipes/$< recipes/$@

# reST+Coq → Coq+reST
output/document.v: document.v.rst | output
	$(alectryon) $< -o $@

# Coq+reST → reST+Coq
output/literate.v.rst: literate.v | output
	$(alectryon) $< -o $@

# JSON → JSON
output/fragments.io.json: fragments.json | output
	$(alectryon) $<

# JSON → HTML snippets
output/fragments.snippets.html: fragments.json | output
	$(alectryon) $< -o $@

# JSON → Latex snippets
output/fragments.snippets.tex: fragments.json | output
	$(alectryon) $< -o $@

# Coq → Latex snippets
output/latex.snippets.tex: latex.v | output
	$(alectryon) --frontend coq $< -o $@

# Sphinx
sphinx/_build/html/index.html: sphinx/index.rst
	+$(MAKE) -C sphinx html

# API
output/api.out: api.py
	python3 $< > $@

output/alectryon%css output/alectryon%js: FORCE
	cp --link --force $(patsubst %,../assets/%,$(assets)) output/
FORCE:

clean:
	rm -rf output/ sphinx/_build sphinx/*.cache

.PHONY: clean assets
