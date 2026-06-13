# MSA Align — Web Edition 🧬

A **single-file, in-browser multiple sequence alignment tool** for amino-acid sequences. No install, no server, no internet required — open `index.html` on any device (phone, tablet, laptop) and it just works.

This is the portable web port of the Python `msa-align-tool`. Because the original relies on the external **MUSCLE** binary (plus BioPython/NumPy and Python 3.11), it can't run on phones or in a browser. This version reimplements the alignment engine in pure JavaScript and reproduces the original's analysis layer and output formats.

## ✨ Features

- **Pure-JavaScript progressive aligner** — pairwise Needleman–Wunsch with **BLOSUM62** and affine gaps → pairwise-distance matrix → **UPGMA guide tree** → progressive profile–profile alignment (a mini-Clustal).
- **Paste or upload** FASTA, or load the bundled example.
- **Colored alignment viewer** — Clustal-style coloring by amino-acid property, a *color-by-conservation* mode, consensus row, and a Clustal conservation track (`*` identical, `:` strong, `.` weak).
- **Analysis** mirroring the original tool: per-column conservation (identity **or** Shannon entropy), consensus sequence, pairwise-identity matrix, gap statistics.
- **Exports**: aligned **FASTA**, **ClustalW** (`.aln`), **PHYLIP** (`.phy`), and a **JSON** analysis report.
- **Works offline** — everything (including the BLOSUM62 matrix) is embedded in one HTML file.

## 🚀 Use it

**Online:** open the GitHub Pages link (Settings → Pages once enabled):
`https://<your-username>.github.io/msa-align-tool/`

**Offline / locally:** download [`index.html`](index.html) and open it in any browser. You can email it to yourself, drop it in iCloud/Drive, or keep it on a USB stick — it has zero dependencies.

## 🧪 How it works

1. **Parse & validate** FASTA (standard 20 amino acids plus ambiguity codes `B J Z X`).
2. **Pairwise distances** — every pair is globally aligned (Gotoh affine NW, BLOSUM62) and distance is `1 − identity`.
3. **Guide tree** — UPGMA clustering over the distance matrix.
4. **Progressive alignment** — profiles are merged in tree order; columns are scored by the average BLOSUM62 substitution score over all non-gap residue pairs.
5. **Analyze & render** — conservation, consensus, identity matrix, gap stats; sequences are re-ordered back to input order for display.

> ⚠️ **Scope.** This is a fast, dependency-free heuristic aligner well suited to small/medium protein families (tested smoothly up to ~60 sequences in the browser; hard cap 200). For publication-grade alignment of large families, use **MUSCLE** or **MAFFT** (e.g. the original `msa-align-tool`).

## 📥 Output formats

| Format | Extension | Notes |
|--------|-----------|-------|
| FASTA | `.fasta` | Aligned sequences with gaps |
| ClustalW | `.aln` | 60-col blocks with conservation symbols |
| PHYLIP | `.phy` | Sequential, 10-char names |
| JSON | `.json` | Summary, conservation scores, consensus, identity matrix, aligned sequences |

## 🔗 Relationship to the Python tool

The original `msa-align-tool` is a Python CLI built around MUSCLE with a clean core/algorithm/analysis/CLI layout. This web edition keeps the **same analysis semantics** (identity & entropy conservation, 0.5-threshold consensus, pairwise-identity matrix, gap percentage) and the **same export formats**, but swaps the MUSCLE call for a self-contained JS engine so it runs anywhere.

## 📄 License

MIT — see [LICENSE](LICENSE).

## 📚 Citation

If the alignment method matters for your work, cite the scoring matrix and the original MUSCLE tool you may compare against:

- Henikoff, S. & Henikoff, J.G. (1992) *Amino acid substitution matrices from protein blocks.* PNAS 89(22), 10915–10919.
- Edgar, R.C. (2004) *MUSCLE: multiple sequence alignment with high accuracy and high throughput.* Nucleic Acids Research 32(5), 1792–1797.
