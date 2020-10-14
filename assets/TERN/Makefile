TARGETS = $(addprefix img/,$(addsuffix .png,$(basename $(wildcard *.plantuml))))

.PHONY: all

all:
	@make ${TARGETS}

img/%.png: %.plantuml
	plantuml -o img/ $<
	pngcrush -new -ow -rem alltext -brute $@ || true
