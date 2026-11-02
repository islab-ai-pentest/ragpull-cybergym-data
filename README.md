# CyberGym Evidence RAG — data downloads

Public data assets for [CyberGym Evidence RAG](https://github.com/islab-ai-pentest/ragpull-cyberx). Download links require no login. The runtime-code repository remains private; this repository publishes data and a reusable index, not a standalone application.

## Download

- [Corpus + raw evidence + completed dense index](https://github.com/islab-ai-pentest/ragpull-cyberx-data/releases/download/openbook-r2/ragpull-openbook-r2.zip) — 834,328,287 bytes, approximately 834 MB ZIP.
- [SHA256SUMS.txt](https://github.com/islab-ai-pentest/ragpull-cyberx-data/releases/download/openbook-r2/SHA256SUMS.txt)
- [Verification and scope](https://github.com/islab-ai-pentest/ragpull-cyberx-data/releases/download/openbook-r2/verification.json)
- [Release page](https://github.com/islab-ai-pentest/ragpull-cyberx-data/releases/tag/openbook-r2)

Archive SHA-256: `094d1ab94284a0a0837f2bb2be88f87919acf7bb601c7bb9948ff56615f08905`.

## Contents

- `curated_data/openbook-r2/`: 14,517 normalized records, 71,764 retrieval chunks, derived Markdown, provenance, original notices, and raw evidence from ARVO-Meta, CyberForge, OSS-Fuzz, Kaitai Struct Formats and FormatFuzzer.
- `curated_data/openbook-r2/raw/mirrors/`: ordinary directories, not Windows junctions. Original raw working-tree bytes are retained; local Git configs, hooks, reflogs and indexes are excluded. Genuine pinned Git objects/HEAD/ref/shallow data and a minimal safe Git config are included for source validation.
- `.cache/level3/dense/`: the completed `openai/text-embedding-3-small` index, 71,645 eligible chunks and 1,536 dimensions. The other 119 chunks remain lexical-only. No corpus re-embedding is required.
- `bundle/inventory.json`: per-file SHA-256 and sizes.
- `bundle/publication-audit.json`: unresolved rights/attribution findings and distribution scope.

Environment files, API response caches, CyberGym task archives and private experiment runs are not included. This does not publish task inputs, model trajectories or PoC execution results. Approximately 1.93 GB is needed for extracted payloads, in addition to the ZIP and runtime checkout.

## Install into an existing runtime checkout

Requires Python 3.12+ and Git on PATH. Use a fresh checkout/data destination: do not extract over an existing completed snapshot/index. After downloading, verify SHA-256 before extraction.

PowerShell:

```powershell
(Get-FileHash .\ragpull-openbook-r2.zip -Algorithm SHA256).Hash.ToLower()
```

Linux/macOS:

```sh
sha256sum -c SHA256SUMS.txt
```

From the root of the runtime checkout:

```sh
python -m zipfile -e ragpull-openbook-r2.zip .
python -m tools.curation.curate validate --output curated_data/openbook-r2
python -m ragpull.evidence --snapshot curated_data/openbook-r2 --query "AddressSanitizer heap buffer overflow invalid read" --top 3
python -m ragpull.evidence --snapshot curated_data/openbook-r2 --record-id arvo:1065 --top 3
```

These validation/lexical/exact commands make no model requests. For hybrid/Jev queries, configure your own `OPENROUTER_API_KEY` using the runtime's `.env.example`; query embedding and Jev can incur API costs:

```sh
python -m ragpull.evidence --snapshot curated_data/openbook-r2 --index-dir .cache/level3/dense --mode hybrid_jev --query "regexec returns success without initializing pmatch" --top 3
```

Never execute downloaded raw projects, build scripts, generators or PoCs on the host. The bundle is evidence for retrieval, not verified executable exploits or a CyberGym solver.

## Licenses and attribution

This is a multi-source aggregate, **not a newly MIT/Apache/BSD-licensed dataset**. Original per-source/per-file copyright, license and notice terms remain applicable. Preserve `notices.json`, original raw notices and `bundle/publication-audit.json`; public availability is not a blanket grant for further redistribution.

**Rights clearance is incomplete.** Known findings include the unresolved RenderWare specification permission in [Kaitai issue #412](https://github.com/kaitai-io/kaitai_struct_formats/issues/412), individual Kaitai/FormatFuzzer terms, and imported third-party code/notice mappings in ARVO, CyberForge and OSS-Fuzz. The complete existing corpus is published at the user's explicit direction without claiming those issues have been resolved. No source was silently removed or relabeled.

## Verification boundary

A separately extracted package passed existing snapshot/output/Markdown/raw hash validation, all five pinned source Git object checks, BM25 and exact lookup, and stored-vector search. All 20,335 referenced raw artifacts verified; 76,868 extracted files were SHA-256 checked. There were zero network/model requests and no re-embedding or PoC execution. Final packaging changed only the two supplemental `bundle/` JSON files, checking every original runtime payload against that smoke-tested package. See `verification.json` for exact identities and limits. Retrieval quality and executable success are not established by packaging checks.
