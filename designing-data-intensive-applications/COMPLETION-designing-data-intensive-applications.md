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

---

## Bản vá 2026-09-12 — section rail

`book-qa` được cập nhật sau wave genesis và thêm ràng buộc **section-index rail**
(scroll-spy), làm fail ngược cả hai trang vốn đã ghi PASS ở trên:

```
- section-index rail CSS missing (no /* SECTION-RAIL:START */ sentinel)
- section-index rail JS missing (no section-rail__list builder)
```

**Đã vá.** Khối CSS (`SECTION-RAIL:START … END`, trước `</style>`) và IIFE
(`section-rail__list`, trước `</script>` cuối) được trích **byte-identical** từ
scaffold canonical `~/Documents/secrets-of-a-super-memory` — bản cover và bản chapter
ở đó giống hệt nhau nên chỉ có một khối duy nhất. Không sửa prose, style khác, hay
nav fence.

| Kiểm chứng | Kết quả |
|---|---|
| `book-qa --kind cover` + `--kind chapter` | ✅ `ALL CHECKS PASS`, exit 0 (cả hai) |
| Khối chèn so với canonical | ✅ identical (diff rỗng, CSS + JS, cả hai trang) |
| Cân bằng tag | ✅ `<style>` 1/1, `<script>` 2/2 mỗi trang |
| Rail render (Chrome 1440×900) | ✅ Ch1: 4 mục → `#reliability`/`#scalability`/`#maintainability`/`#diagnostic`; bìa: 2 mục → `#parts`/`#how-to-read` |
| Scroll-spy | ✅ 4/4 section map đúng chỉ số active |
| Ẩn ở viewport hẹp | ✅ `display: none` tại 1100px (ngưỡng 1280px) |
| Chín biến CSS rail phụ thuộc | ✅ đủ trong cả light lẫn dark |

**Phạm vi:** đây là lần chạy lại **gate `book-qa` + kiểm chứng render**, *không* phải
một vòng `book-fidelity-auditor` mới. Các dòng ✅ ở bảng đầu (link sweep, external
refs, faithfulness) vẫn dựa trên lần audit genesis — nội dung trang không đổi nên
chúng không bị bản vá này làm mất hiệu lực.

---

# Wave Phần I — Chương 2–4 (2026-09-12)

Tác giả: ba `book-chapter-author` chạy song song. Tích hợp: `book-cover-curator`.
Thẩm định: `book-fidelity-auditor` (read-only). Gate: `book-qa`.

| Trang | book-qa | Scaffold | Faithfulness (nguồn / spot-check) | Nav state |
|---|---|---|---|---|
| index.html (bìa) | ✅ PASS (`--kind cover`) | ✅ | n/a — không có cite trong trang; roster "4/4" khớp thực tế trên đĩa | Ch1–4 live; Phần II/III đúng "sắp có" |
| chapter-1-…applications.html | ✅ PASS (template = ch2) | ✅ nội dung không đổi (chỉ NAV + ghi chú ref) | 8/8 ref đúng như đã ghi: O'Reilly/CACM 403, InfoQ 405 — bot-blocked, không phải link chết | prev → Bìa; next → Ch2 (live) |
| chapter-2-data-models-and-query-languages.html | ✅ PASS (template = ch1) | ✅ | 24/24 ref resolve; spot-check 4 đặc thù — IMS/Apollo 1968 (khớp IBM history), CODASYL suy tàn đầu 1980s, định nghĩa impedance mismatch, MongoDB khai tử map-reduce từ 5.0 | prev → Ch1; next → Ch3 |
| chapter-3-storage-and-retrieval.html | ✅ PASS (template = ch1) | ✅ | 25/25 ref resolve; auditor tải thẳng PDF gốc O'Neil 1996 — câu trích khớp **nguyên văn**; "LSM luôn nhanh hơn B-tree" được đóng khung đúng là ngộ nhận; leveled/size-tiered có box "bối cảnh 2017", nêu cả UCS 5.0; số LevelDB/RocksDB khớp source | prev → Ch2; next → Ch4 |
| chapter-4-encoding-and-evolution.html | ✅ PASS (template = ch1) | ✅ | 26/26 ref resolve; công thức tag/zigzag/wire-type khớp protobuf.dev nguyên văn; cặp 59/34 byte khớp blog Kleppmann 2012 **và** được đóng khung rõ là đo trên một bản ghi ví dụ, không phải tỉ lệ nén phổ quát; `required` vắng mặt trong proto3 khớp doc hiện hành; **không** có khẳng định nào về gRPC streaming | prev → Ch3; next → Ch5 disabled "sắp có" |
| chapter-5 … chapter-12 | — | — | — | sắp có (chưa viết) |

**Site-level**
- Link/anchor/cite: ✅ 0 link nội bộ chết, 0 anchor `#id` treo, mọi cite `#ref-N` resolve đúng trang. Chuỗi nav Ch1↔Ch2↔Ch3↔Ch4 đúng hai chiều; Ch4→Ch5 đúng là span "sắp có".
- External refs (sweep độc lập, UA Chrome thật): **76 URL duy nhất** → 71×200, 4×403 (cacm.acm.org, oreilly.com, 3× w3.org), 1×405 (infoq.com). **Zero 404/NXDOMAIN** — không có link nào thực sự chết đội lốt bot-blocked.
- Bilingual: ✅ đủ 4 lớp, không có `.quick-only`; 0 phần tử `en-only` thiếu `lang="en"`. Kiểm bằng parser DOM thật. Ngoại lệ duy nhất: khối `trap-comparison deep-only` chỉ có standard+en — đúng thiết kế, vì nó đã bị `deep-only` che ở chế độ dễ. Script + CSS mode-switch byte-identical trên cả 5 trang. Storage key nhất quán 5/5.
- Curator-region: ✅ không leak. Baseline `e93d8d5`. Ch1 = 2 hunk, cả hai nằm trọn trong NAV fence; `index.html` = roster/counts/intro, đúng phạm vi vai trò.
- Faithfulness caveat: **spot-check, không phải audit toàn diện** — 4 đặc thù/trang cho Ch2–4, fetch thẳng nguồn gốc chứ không tin text ref-list; Ch1 dựa một phần vào spot-check genesis vì nội dung không đổi.

**Verdict: ✅ PASS** — cho phép đánh ✅ các dòng trên.

## Quy ước mới: đánh dấu bot-blocked ngay trong ref-list

Ref trả 403/405 vì chặn bot giờ mang một `<em>` anh em **ngoài** thẻ `<a>`, ghi rõ host nào chặn và mã trả về. Mục đích: trang tự đứng được — người verify bấm link gặp 403 mà không có ghi chú thì không phân biệt được chặn bot với link chết, và không phải ai cũng lục lại file này.

Ch2 và Ch4 tự đặt quy ước trong wave này; auditor khuyến nghị **đồng bộ Chương 1 theo quy ước mới** thay vì gỡ nó khỏi Ch2/Ch4. Đã thi hành: `ref-3` (oreilly 403), `ref-4` (infoq 405), `ref-5` (cacm 403) của Chương 1 nay có ghi chú; cả 5 trang chạy lại `book-qa` vẫn PASS. Ch2 để ghi chú **bên trong** `<a>`, khác Ch4 và Ch1 — khác biệt hình thức đã biết, không chặn PASS.

**Khác biệt đã biết, chưa xử lý:** `ref-1` (trích dẫn chính cuốn sách) là text trơn ở Ch1/Ch3/Ch4 nhưng được Ch2 link tới Open Library. Không vi phạm "không bịa" — trích dẫn sách in vẫn đầy đủ tên/nhà xuất bản/năm/chương dù không có URL. Để lại cho lượt polish sau.

## Tái tạo QA

```
cd designing-data-intensive-applications
book-qa index.html chapter-2-data-models-and-query-languages.html --kind cover
book-qa chapter-1-reliable-scalable-and-maintainable-applications.html chapter-2-data-models-and-query-languages.html --kind chapter
for f in chapter-2-*.html chapter-3-*.html chapter-4-*.html; do
  book-qa "$f" chapter-1-reliable-scalable-and-maintainable-applications.html --kind chapter
done
```

⚠️ Đừng dùng chính một trang làm template cho nó — `book-qa X X` luôn sinh finding giả *"template `<title>` still present"*.

---

# Wave Phần II — Chương 5–9 (2026-09-12)

Tác giả: năm `book-chapter-author` chạy song song (ch5–9). Tích hợp: `book-cover-curator`.
Thẩm định: `book-fidelity-auditor` (read-only). Gate: `book-qa`.

| Trang | book-qa | Scaffold | Faithfulness (nguồn / spot-check) | Nav state |
|---|---|---|---|---|
| index.html (bìa) | ✅ PASS (`--kind cover`) | ✅ | n/a — không cite; roster "5/5" khớp thực tế | Thẻ Phần II từ `soon` → live, 5/5 link đúng |
| chapter-1-…applications.html | ✅ PASS | ✅ | 8/8 ref resolve; lượt `ddia-ch1-fix` thêm cite `ref-1` (0 → 56) và `ref-3` (0 → 3); không xoá cite ngoài nào | prev → Bìa; next → Ch2 |
| chapter-2-data-models-and-query-languages.html | ✅ PASS | ✅ | 24/24 ref resolve; không đổi so với wave trước | không đổi |
| chapter-3-storage-and-retrieval.html | ✅ PASS | ✅ | 25/25 ref resolve | NAV: chính tả "hoá"→"hóa" trong fence |
| chapter-4-encoding-and-evolution.html | ✅ PASS | ✅ | 26/26 ref resolve | NAV: next Ch5 disabled-span → live |
| chapter-5-replication.html | ✅ PASS | ✅ | 25/25 ref resolve; `w + r > n` đặt tên "điều kiện, không phải bảo đảm" + liệt kê 5 trường hợp công thức đúng mà dữ liệu vẫn sai; LWW đặt tên "hội tụ bằng cách vứt dữ liệu đi" | prev → Ch4; next → Ch6 |
| chapter-6-partitioning.html | ✅ PASS | ✅ | 27/27 ref resolve; "consistent hashing" đóng khung đúng phân biệt Karger 1997 vs cách dùng lỏng trong CSDL, theo khuyến nghị gọi `hash partitioning` | prev → Ch5; next → Ch7 |
| chapter-7-transactions.html | ✅ PASS | ✅ | 20/20 ref resolve; tên mức cô lập cite đúng doc từng vendor — PostgreSQL "repeatable read" = SI verified nguyên văn qua WebFetch; Oracle "serializable" = SI qua Hermitage; MySQL yếu hơn qua doc MySQL | prev → Ch6; next → Ch8 |
| chapter-8-the-trouble-with-distributed-systems.html | ✅ PASS | ✅ | 22/22 ref resolve; mọi số liệu có khối "Về các con số" nêu điều kiện đo; GC 4,17s/11,45s khớp nguyên văn blog LinkedIn; sự cố GitHub verified — GitHub viết *"a minute and a half"*, nên khung "90 giây là cách đọc của Kleppmann" là chính xác | prev → Ch7; next → Ch9 |
| chapter-9-consistency-and-consensus.html | ✅ PASS | ✅ | 31/31 ref resolve; CAP đúng phạm vi hẹp (C = chỉ khả tuyến tính hoá, mô hình một thanh ghi, lỗi duy nhất = phân vùng mạng, "2 trong 3" gọi thẳng là gây hiểu lầm); linearizability ≠ serializability tách bạch bằng ví dụ PostgreSQL SSI; FLP nêu đúng điều kiện | prev → Ch8; next → Ch10 disabled "sắp có" |
| chapter-10 … chapter-12 | — | — | — | sắp có (chưa viết) |

**Site-level**

- Link/anchor/cite: ✅ 0 link nội bộ chết, 0 anchor treo, 0 cite treo trên cả 10 trang; cite-count = ref-count khớp từng trang. Chuỗi nav Ch1↔…↔Ch9 đúng hai chiều.
- External refs: **107 URL duy nhất** trong Ch5–9 → **0 link chết thật** (404/NXDOMAIN). 97×200, 8×403 bot-blocked, 1×502, 2×429 (rate-limit của chính script kiểm, retry ra 200).
- Bilingual: ✅ đủ 4 lớp, 0/10 trang có `.quick-only`; **0/848** phần tử `en-only` thiếu `lang="en"` (kiểm bằng parser DOM thật).
- Script & style: hai khối `<script>` **byte-identical** trên cả 10 trang kể cả bìa; `<style>` chỉ khác watermark `DDIA / 0N`. Storage key khớp 10/10.
- Curator-region: ✅ không leak. Baseline `b2ac565`. Diff Ch1 (112 dòng) xác nhận là lượt `ddia-ch1-fix`, không phải curator.
- Faithfulness caveat: **spot-check, không phải audit toàn diện** — 12 đặc thù rủi ro cao được lấy mẫu trên 5 chương; 4 verified độc lập qua WebFetch tới tận nguồn gốc (doc PostgreSQL, blog LinkedIn, blog Kleppmann + báo cáo sự cố gốc của GitHub), 8 còn lại xác nhận qua đọc on-page với cite resolve được.

**Verdict: ✅ PASS** — cho phép đánh ✅ các dòng trên.

## Bốn việc polish sau PASS (đã xong)

Auditor nêu 4 gap không chặn PASS; cả bốn đã được route về đúng agent và xử lý xong:

| # | Vấn đề | Xử lý |
|---|---|---|
| 1 | Ch5 `ref-18` (haslab.wordpress.com, 403) thiếu ghi chú bot-block | Đã thêm `<em>` ngoài thẻ `<a>` |
| 2 | Ch7 `ref-10` (dev.mysql.com, 403) thiếu ghi chú | Đã thêm; Ch7 nay có 4 ref bị chặn, tất cả đều có ghi chú |
| 3 | Ch6 `ref-20` (elastic.co/blog) trả 502 | **Link mục nát thật** (502 trên 6 lần thử; `elastic.co/docs` vẫn 200). Thay bằng doc `_routing` chính thức (200), chặt hơn blog cũ. Một khẳng định chuyển sang `ref-21` thật sự đỡ được nó; **hai mảnh chỉ blog mới có — con số "cả 20 shard" và một câu trích — bị gỡ hẳn** thay vì bịa nguồn |
| 4 | Ch7 dùng "phân vùng" cho sharding trong khi canonical là "Phân mảnh" | Đổi đủ 9/9 chỗ; Ch7 nay còn 0 chữ "phân vùng". "Phân vùng **mạng**" ở Ch5/Ch8/Ch9 là khái niệm khác, giữ nguyên |

## Bài học hạ tầng của wave này

**Mã HTTP phụ thuộc User-Agent theo cách phản trực giác.** Kết luận "403 = chặn bot" đo bằng một UA duy nhất là không đáng tin:

| Host | UA Chrome | `Mozilla/5.0` | `curl` | Chrome thật |
|---|---|---|---|---|
| haslab.wordpress.com | 403 | 200 | 200 | ✅ mở được |
| dev.mysql.com | 403 | 403 | **200** | ✅ mở được |
| w3.org | 403 | 403 | 403 | ✅ qua Cloudflare |
| elastic.co/blog | 502 | 502 | 502 | ❌ hỏng thật |

Chỉ khi mở bằng **trình duyệt thật** mới phân biệt được ba tình huống khác hẳn nhau: chặn bot mà người đọc vẫn vào được, thử thách Cloudflare, và link mục nát. Đã đưa quy trình này vào `CLAUDE.md`.

**Ref trỏ blog của vendor mục nát nhanh hơn ref trỏ doc của vendor.** Ưu tiên doc chính thức khi cả hai cùng đỡ được một khẳng định.

**Scratchpad dùng chung gây ghi đè chéo.** Năm agent ghi file trung gian **cùng tên** (`body1.html`…) vào **cùng một thư mục**; bản ráp đầu của Ch5 lẫn nội dung Ch7 và Ch9. `ddia-ch5` tự phát hiện, chuyển sang thư mục riêng, viết lại, và cảnh báo. Kiểm chéo sau đó (định danh chương; thuật ngữ chỉ-có-ở-một-chương; nội dung từng section khớp id; auditor đọc hiểu độc lập) xác nhận **không có nhiễm nào sống sót** — nhưng mọi gate đều xanh trong lúc file đang lẫn, nên nếu Ch5 không tự bắt thì cả wave đã hỏng âm thầm. Đã đưa luật `scratchpad/chN/` vào `CLAUDE.md`.

## Mục mở

**Chính tả `hoá` / `hóa` trộn lẫn toàn cuốn** — có sẵn từ wave genesis, không do wave này sinh ra. Bìa + Ch1 + Ch4 dùng `hóa` (60 chỗ); Ch2, 3, 5, 6, 7, 8, 9 dùng `hoá` (436 chỗ). Cả hai đều là tiếng Việt hợp lệ, khác nhau ở chỗ đặt dấu thanh. Tên chương canonical trên bìa và toàn bộ nav dùng `hóa`. Chưa chuẩn hoá — chờ quyết định, cần một lượt riêng.

**`ref-1` thiếu link ngoài** ở 8/9 chương (chỉ Ch2 link tới Open Library). Không vi phạm "không bịa" (trích dẫn sách in đủ tên/nhà xuất bản/năm/chương). Để lượt polish sau.
