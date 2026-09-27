# questionbank-editor

Web editor tĩnh (GitHub Pages) để xem / sửa gói câu hỏi
`smarttest-upload/submit/questions_{vn,en}.json` của repo private
[nhphung1964/QuestionBank](https://github.com/nhphung1964/QuestionBank)
(môn CO3005 — Nguyên lý Ngôn ngữ Lập trình, HK261).

**Trang chủ:** <https://nhphung1964.github.io/questionbank-editor/>

## Dùng

1. Mở trang, dán **GitHub PAT** vào ô trên đầu (fine-grained, repo
   `nhphung1964/QuestionBank`, quyền **Contents: Read and Write**).
   Token chỉ nằm trong `localStorage` máy bạn, gửi thẳng tới
   `github.com` / `raw.githubusercontent.com` — trang tĩnh này không có
   backend, không lưu dữ liệu đề.
2. Chọn file ở góc phải (`questions_vn.json` / `questions_en.json`). Cây bên trái
   tổ chức theo **chương (C1 INTRO → C6 AST) → chủ đề (63) → câu/cụm**, dựa trên
   metadata (`parentTopic`, `topicCode`, `topicName`) trong file JSON; có ô tìm
   kiếm (khi gõ, cây chuyển thành danh sách phẳng kèm nhãn chương · chủ đề) và
   bộ lọc "chỉ bản ghi lỗi" theo QC.
3. Mỗi lần xem = **1 câu đơn hoặc trọn cụm** (stem + các câu con của nó):
   - **👁 Xem** (mặc định): render dạng đề thi — stem, rồi từng câu con với
     đáp án; code mono đúng xuống dòng, đáp án đúng tô xanh.
   - **✏️ Sửa**: textarea HTML + preview song song cho thân câu và từng đáp án
     của mọi bản ghi trong cụm; radio đổi đáp án đúng.
   - Mỗi bản ghi có panel md nguồn (`part_1_{vn,en}.md`) để đối chiếu backport.
4. Lưu: **Commit lên GitHub** (Contents API — 1 commit cho mọi thay đổi trong
   cụm, message liệt kê đúng các bản ghi đã sửa) hoặc **Tải bản đã sửa** rồi
   commit tay.

## Tính năng QC

- Cảnh báo code nghi bị "phẳng" thành inline (`<code>` chứa `->`, `|`,
  `;`, nhiều space — dấu hiệu BNF/ANTLR mất xuống dòng).
- `htmlContent` chứa `\n` literal, `<p>` lẻ, số đáp án đúng ≠ 1,
  số đáp án < 4.
- Nút "Quét QC toàn bộ" lọc ra các bản ghi cần sửa.

## Vì sao format JSON giữ nguyên khi lưu?

`convert.py` ghi `json.dumps(records, ensure_ascii=False, indent=1)`
(không newline cuối). Đã kiểm chứng `JSON.stringify(v, null, 1)` của
trình duyệt sinh **byte-identical** với dữ liệu này — commit từ trang
không tạo diff giả.
