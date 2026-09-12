# backend-bookshelf — hướng dẫn làm việc

Kệ sách back end: **một repo, nhiều cuốn**. Mỗi cuốn là một site companion song ngữ
VI/EN tự chứa, không framework, một file HTML mỗi chương. Bố cục thư mục và mô tả
chung nằm ở `README.md` — file này chỉ nói **cách làm việc trong repo**.

## Luật tối cao: "không bịa"

Mọi khẳng định đặc thù — số liệu, trích dẫn, tên người, ví dụ đích danh — phải có
cite `[N]` trỏ tới một mục ref-list có link ngoài **thật, resolve được**. Không có
tham chiếu thì không lên trang. Ví von trong chế độ *Dễ hiểu* là minh hoạ bổ sung,
được phép, nhưng không được trình bày như ví dụ nguyên văn của tác giả.

Khi một con số là số **lịch sử** (trích từ một bài nói/bài báo cũ, không phải số vận
hành hiện thời), footer chương phải nói rõ điều đó. Xem caveat Twitter ở Chương 1
DDIA làm mẫu.

## Routing — dùng agent, đừng sửa tay

| Việc | Agent | Chế độ chạy |
|---|---|---|
| Viết / thêm một trang chương | `book-chapter-author` | **song song**, một agent một chương |
| Tích hợp bìa + nav sau một wave | `book-cover-curator` | **tuần tự**, không bao giờ chạy cùng author |
| Verify toàn site + báo cáo | `book-fidelity-auditor` | read-only, chạy cuối |

Trang không phải chương (mind-map, trang Part, glossary) hiện vẫn là việc của main
session — author chưa tổng quát hoá cho chúng.

**Hai điểm cứng, còn lại là khuyến nghị:**
- `book-cover-curator` **từ chối** nếu `<book>/index.html` chưa tồn tại — genesis phải
  chạy trước.
- Một trang/bộ chỉ "xong" khi `book-fidelity-auditor` trả **PASS**. PASS đó mới cho
  phép đánh ✅ trong `COMPLETION-<slug>.md`. Auditor không ghi file — main session
  commit báo cáo từ output nó trả về.

## Gate: `book-qa`

`book-qa` (trên `$PATH`, deploy từ `nix-config`) là gate canonical duy nhất. Cả ba
agent đều gọi nó — **không bao giờ nhúng bản copy**.

```
cd <book-slug>
book-qa index.html <một-chương-bất-kỳ>.html --kind cover
book-qa <chương>.html index.html            --kind chapter
```

Cả hai phải in `ALL CHECKS PASS` (exit 0). Đối số thứ hai là trang template dùng để
đối chiếu localStorage key + title.

> `book-qa` được cập nhật độc lập với repo này. Một `COMPLETION` ghi PASS **không**
> chứng minh hôm nay còn PASS — chạy lại gate trước khi tin vào nó. Ngày 2026-09-12
> checker thêm ràng buộc section-rail và làm fail ngược hai trang genesis vốn đã
> được ghi PASS.

## Bất biến của mỗi trang

Scaffold phải được copy **nguyên văn**, không cắt gọt. Những thứ dưới đây là bất biến
— thiếu một cái là fail gate hoặc vỡ trải nghiệm đọc:

- **Section rail** (scroll-spy bên trái): khối CSS giữa
  `/* SECTION-RAIL:START */ … /* SECTION-RAIL:END */` đặt **ngay trước `</style>`**, và
  IIFE `/* SECTION-RAIL: … */` chứa `section-rail__list` đặt **ngay trước `</script>`
  cuối cùng**. Rail tự dựng từ `main .section[id]` + `main .closing[id]`, cần **≥ 2**
  section có `id` mới hiện; ẩn dưới 1280px. Không có markup per-page nào cả.
- **Bốn lớp song ngữ**: `.vi-only` / `.en-only` (mọi phần tử `en-only` phải mang
  `lang="en"`), và ba chế độ đọc VI qua `data-reading-mode` = `easy` | `quick` | `deep`.
  Chỉ có **ba** class, không có `.quick-only`: `.easy-only` hiện riêng ở *easy*,
  `.standard-copy` hiện ở *quick* + *deep*, `.deep-only` hiện riêng ở *deep*. Tức
  *quick* = `.standard-copy` trừ đi `.deep-only`.
- **Khối chống nháy** ở `<head>`: IIFE đọc localStorage và set `data-lang` +
  `data-reading-mode` trước khi render.
- **Storage key theo cuốn**: `<prefix>-lang` và `<prefix>-reading-mode`, giống nhau
  trên mọi trang của cùng một cuốn (DDIA: `ddia-lang` / `ddia-reading-mode`). Kệ gốc
  dùng riêng `shelf-lang`.
- **NAV fence**: mọi sửa đổi điều hướng chỉ được nằm trong
  `<!-- NAV:START … NAV:END -->`. Author link neighbour một cách lạc quan; curator là
  bên duy nhất được demote về `<span>` disabled "sắp có".
- **Ghi chú bot-blocked**: ref trả 403/405 vì host chặn fetcher (w3.org, oreilly.com,
  cacm.acm.org, infoq.com) vẫn được cite bình thường — chúng có thật, chỉ chặn bot.
  Kèm một `<em>` anh em **ngoài** thẻ `<a>` ghi rõ host nào chặn và mã trả về, để
  trang tự đứng được mà không cần người đọc lục file này.

**Nguồn scaffold canonical:** `~/Documents/secrets-of-a-super-memory` (cùng khuôn,
đã mang đủ rail). Khi cần một khối bị thiếu, trích từ đó chứ đừng viết lại — khối
rail ở bản cover và bản chapter là byte-identical.

## Chủ quyền file

| File | Ai sở hữu |
|---|---|
| `index.html` (gốc, kệ) | **main session** — curator không đụng tới |
| `<book>/index.html` (bìa) | `book-cover-curator` |
| `<book>/chapter-N-*.html` | `book-chapter-author` viết; sau đó chỉ NAV fence là của curator |
| `<book>/COMPLETION-<slug>.md` | main session commit, nội dung từ auditor |

Sau khi một cuốn qua gate, nhớ sync card của nó trên kệ gốc: blurb, CTA và số chương
đã sẵn sàng.

## Palette theo cuốn

Mỗi cuốn có bảng màu riêng, khai báo ba lần trong `:root` / `@media
(prefers-color-scheme: dark) :root:not([data-theme="light"])` / `:root[data-theme="dark"]`.
DDIA dùng tông giấy cũ: `--teal: #b5502f` (đất nung), `--blue: #2b7c73`,
`--amber: #b07d1a`, `--leaf: #4f7a52`. Rail phụ thuộc vào `--surface-strong`,
`--surface`, `--line`, `--shadow`, `--teal`, `--teal-soft`, `--ink`, `--muted`,
`--body` — cuốn mới phải định nghĩa đủ chín biến này.

## Trạng thái các cuốn

| Cuốn | Chương | Ghi chú |
|---|---|---|
| `designing-data-intensive-applications` | **4 / 12** | Kleppmann, O'Reilly 2017. Phần I **4/4 trọn vẹn**; Phần II 0/5; Phần III 0/3. Mind-map: CSS `.mindmap-banner` đã có, chưa có element trong body. |

Việc kế tiếp của DDIA: Chương 5–9 (Phần II · Distributed Data).
