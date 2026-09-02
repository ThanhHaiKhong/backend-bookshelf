# backend-bookshelf

Kệ sách back end — mỗi cuốn là một site companion song ngữ (VI/EN) tự chứa, không
framework, một trang HTML mỗi chương. Trang `index.html` gốc là **kệ**: liệt kê mọi
cuốn đã dựng. Cùng khuôn với [`flow`](https://github.com/ThanhHaiKhong/flow) và
[`thinking-in-system`](https://github.com/ThanhHaiKhong/thinking-in-system), khác ở
chỗ đây là **một repo chứa nhiều cuốn** (kệ theo chủ đề).

A back-end bookshelf — each book is a self-contained bilingual (VI/EN) companion
site (no framework, one HTML page per chapter). The root `index.html` is the
**shelf** linking to every book built so far.

## Cấu trúc / Layout

```
backend-bookshelf/
├── index.html                 # KỆ: card link tới từng cuốn (main session giữ)
├── <book-slug>/               # mỗi cuốn = 1 site tự chứa
│   ├── index.html             #   bìa cuốn (book-cover-curator sở hữu)
│   ├── chapter-1-….html …     #   một trang mỗi chương
│   ├── mind-map.html          #   tổng hợp toàn cuốn (tuỳ chọn)
│   ├── COMPLETION-<slug>.md    #   báo cáo hoàn tất (auditor PASS)
│   └── CHECKLIST.md            #   tiêu chí
└── …
```

## Quy trình mỗi cuốn / Per-book pipeline

`book-chapter-author` (song song, một chương một file) → `book-cover-curator`
(tuần tự, tích hợp bìa + nav trong cuốn) → `book-fidelity-auditor` (read-only,
done-gate). Sau khi một cuốn PASS, thêm card của nó vào kệ `index.html` gốc.

**Luật tối cao: "không bịa"** — mọi số liệu/trích/ví dụ đích danh phải có cite `[N]`
trỏ tới nguồn ngoài thật. Không có tham chiếu thì không lên trang.

## Đọc / Read

GitHub Pages: <https://thanhhaikhong.github.io/backend-bookshelf/>
