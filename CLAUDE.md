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
- **Trích dẫn phải trỏ đúng nguồn**: `ref-1` là **cuốn sách**, `ref-2` là **trang web**
  của sách. Mọi phát biểu quy cho tác giả ("Kleppmann nói…", cách ông đóng khung vấn
  đề, ví dụ ông dùng) phải cite `ref-1`, không phải `ref-2`. Đây là lỗi đã xảy ra ở
  **4/5 agent** trong một wave và `book-qa` không bắt được. Mật độ tham chiếu: 33–62
  cite `ref-1` mỗi chương.
- **0 ref thừa, 0 cite treo**: mọi mục trong ref-list phải được cite ít nhất một lần,
  và mọi cite phải trỏ tới một mục có thật. Ref chỉ dùng để định danh ấn bản thì cite
  vào dòng nguồn-chính ở footer.

**Nguồn scaffold canonical:** `~/Documents/secrets-of-a-super-memory` (cùng khuôn,
đã mang đủ rail). Khi cần một khối bị thiếu, trích từ đó chứ đừng viết lại — khối
rail ở bản cover và bản chapter là byte-identical.

## Kiểm link ngoài — đừng tin một phép đo

Mã HTTP **phụ thuộc User-Agent theo cách phản trực giác**: cùng một URL có thể trả 403
cho UA Chrome đầy đủ nhưng 200 cho `curl` trần, vì bộ lọc bot phạt thứ *giả dạng*
trình duyệt. Đo bằng một UA rồi kết luận là sai.

Quy trình trước khi dán nhãn "bot-blocked" hay tuyên bố một link đã chết:

1. Thử ít nhất hai User-Agent (UA Chrome đầy đủ **và** `curl` trần).
2. Nếu còn nghi ngờ, mở bằng **trình duyệt thật** (`chrome-devtools-axi open <url>`
   rồi đọc `document.title`). Chỉ cách này mới phân biệt được ba tình huống khác hẳn
   nhau: chặn bot mà người đọc vẫn vào được · thử thách Cloudflare · link mục nát.
3. **5xx không phải chặn bot.** Không được ghi chú kiểu bot-blocked để cho qua. Thay
   bằng nguồn còn sống, hoặc bỏ hẳn khẳng định nếu không có nguồn nào đỡ được nó.

**Ưu tiên doc chính thức hơn blog của cùng vendor** — blog mục nát nhanh hơn nhiều.

## Fan-out song song — mỗi agent một thư mục scratchpad

Agent viết chương hay dùng tên file trung gian giống nhau (`body1.html`…). Khi nhiều
agent chạy song song trong **cùng một thư mục** scratchpad, chúng **ghi đè lẫn nhau**
và ráp ra trang lẫn nội dung của chương khác — trong khi `book-qa`, cite/ref, rail,
nav đều vẫn xanh, vì đó là lỗi *nội dung* chứ không phải *cấu trúc*.

Luật: mỗi agent trong một đợt fan-out dùng thư mục con riêng — `scratchpad/ch5/`,
`scratchpad/ch6/`… — hoặc tiền tố tên file theo chương.

Sau mỗi wave song song, kiểm nhiễm chéo trước khi tin vào gate: định danh chương
(`<title>`, `content: "DDIA / 0N"`, `.book-label`, tập `section[id]`), và các thuật
ngữ **chỉ thuộc về một chương** (đếm từ khoá chung như "quorum" sẽ báo động giả).

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
| `designing-data-intensive-applications` | **9 / 12** | Kleppmann, O'Reilly 2017. Phần I **4/4** và Phần II **5/5** trọn vẹn; Phần III 0/3. Mind-map: CSS `.mindmap-banner` đã có, chưa có element trong body. Mục mở: chính tả `hoá`/`hóa` trộn lẫn toàn cuốn (436 vs 60), chưa chuẩn hoá. |

Việc kế tiếp của DDIA: Chương 10–12 (Phần III · Derived Data).
