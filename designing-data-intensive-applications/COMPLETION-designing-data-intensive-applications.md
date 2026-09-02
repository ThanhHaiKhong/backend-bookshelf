# COMPLETION — Designing Data-Intensive Applications (VI/EN companion)

Book: Martin Kleppmann, *Designing Data-Intensive Applications*, O'Reilly Media, 2017.
Shelf: backend-bookshelf · Wave: genesis (cover + Chapter 1).
Verifier: book-fidelity-auditor (read-only). QA gate: `book-qa`.

| Page | book-qa | Scaffold | Faithfulness (sources / flags) | Nav state |
|------|---------|----------|--------------------------------|-----------|
| index.html (cover) | ✅ PASS (`--kind cover`) | ✅ cover scaffold | n/a — cover exempt from ref-list/external checks; no stray numeric claims | Card → Chapter 1 live |
| chapter-1-reliable-scalable-and-maintainable-applications.html | ✅ PASS (`--kind chapter`) | ✅ verbatim chapter scaffold | ✅ 8 refs, all resolve; specifics cited (Twitter→ref-4, taxonomy/humans→ref-2, Chaos Monkey→ref-6, chaos eng→ref-7, tail latency→ref-5, SLO→ref-8); footer Twitter disclaimer correct | prev → Cover (live); next → Ch2 disabled span "sắp có" |
| chapter-2 … chapter-12 | — | — | — | sắp có (not yet authored) |

**Site-level**
- Link sweep: ✅ zero dead local links (2 files, both hrefs resolve; ch2 is a disabled no-href span). Anchors: ✅ all `#id` resolve. Cites: ✅ all inline `#ref-N` resolve to `<li id>`.
- External refs: ✅ 8/8 resolve — ref-2/6/7/8 → 200; ref-3 (O'Reilly) 403, ref-4 (InfoQ) 405, ref-5 (CACM) 403 = exists-but-bot-blocked, acceptable.
- Bilingual: ✅ four layers present & balanced; all en-only carry `lang="en"`; storage keys `ddia-lang` / `ddia-reading-mode` consistent across pages.
- Curator-region: not run (no baseline — directory untracked, genesis wave).
- Faithfulness caveat: spot-check of 7 specifics on the chapter page (not an exhaustive audit); high-stakes numeric/attribution claims verified correctly framed and cited; ref-5 confirmed as real-but-bot-blocked, claim faithful.

**Verdict: ✅ PASS** — both content pages pass book-qa; zero dead links/anchors/cites; no unsourced or invented specific in the sample. This PASS authorises the ✅ rows above. Chapters 2–12 remain to be authored.

---

## Tái tạo QA / Reproduce the gate

```
cd designing-data-intensive-applications
book-qa index.html chapter-1-reliable-scalable-and-maintainable-applications.html --kind cover
book-qa chapter-1-reliable-scalable-and-maintainable-applications.html index.html --kind chapter
```

Cả hai phải in `ALL CHECKS PASS`.

## Nguồn Chương 1 / Chapter 1 sources
1. Kleppmann, *Designing Data-Intensive Applications*, O'Reilly 2017 — Ch. 1 (nguồn chính).
2. <https://dataintensive.net/> — trang chính thức; bộ ba reliable/scalable/maintainable, khung "data-intensive".
3. <https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/> — trang sách O'Reilly (mục lục, định danh chương).
4. <https://www.infoq.com/presentations/Twitter-Timeline-Scalability/> — Krikorian, "Timelines at Scale", QCon 2012; số Twitter & fan-out.
5. <https://cacm.acm.org/research/the-tail-at-scale/> — Dean & Barroso, "The Tail at Scale", CACM 2013; tail latency/percentile.
6. <https://github.com/Netflix/chaosmonkey> — Chaos Monkey.
7. <https://principlesofchaos.org/> — Principles of Chaos Engineering.
8. <https://sre.google/sre-book/service-level-objectives/> — Google SRE Book, SLI/SLO/SLA.

**Caveat trung thực:** số Twitter (~4.6k/12k/300k req/s) là số Kleppmann trích từ bài nói 2012 của Krikorian (ref-4), **không** phải số vận hành hiện thời — footer chương ghi rõ điều này. Ví von chế độ Dễ hiểu (bánh xe dự phòng, con ốc lỏng…) là minh hoạ bổ sung, không phải ví dụ nguyên văn trong sách.
