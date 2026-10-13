# Gợi ý nâng cấp — rà soát 2026-10-13

> Lấy từ đợt rà soát chéo 19 repo của chủ sở hữu (ma trận vệ sinh ở [seeker19110/stored, mục 23 §8.1](https://github.com/seeker19110/stored/blob/main/23-own-repos-lessons/README.md); gợi ý tích hợp ở §7b của cùng file). Số liệu đo bằng lệnh trên HEAD (đếm `uses:` ghim SHA 40 ký tự, khối `permissions:`, `dependabot.yml`, `.claude/settings.json`), không suy ra từ văn xuôi. Đây là **đề xuất**, chưa sửa code. Mỗi mục kèm bằng chứng để kiểm lại trước khi làm; mục nào đã xử lý thì đánh dấu hoặc xoá.

## Hiện trạng đo được

| HEAD đọc | Action ghim SHA | Workflow thiếu `permissions:` cấp đầu | Dependabot | LICENSE · SECURITY | File test |
|---|---|---|---|---|---|
| `24d7968` (2026-08-29) | 2/12 | `codeql.yml`, `skill-optimize-apply.yml`, `skill-preview.yml` | **không có** | có · có | 0 |

## Đề xuất (🔴 cao · 🟠 trung bình · 🟢 nhẹ)

| # | Mức | Đề xuất | Bằng chứng | Kiểm xong khi |
|---|---|---|---|---|
| 1 | 🔴 | Giới hạn người được kích hoạt `/apply-optimize`: thêm điều kiện `contains(fromJSON('["OWNER","MEMBER","COLLABORATOR"]'), github.event.comment.author_association)` vào `if:` của `skill-optimize-apply.yml` | Workflow chạy trên `issue_comment` với `contents: write` + `pull-requests: write` và chỉ kiểm nội dung comment, nên ai comment được trên PR đều kích hoạt được. Lỗi **kế thừa nguyên từ upstream** SnailSploit/claude-red (HEAD `a8de813` vẫn như vậy); nên báo cho upstream luôn | Comment từ tài khoản ngoài không kích hoạt job |
| 2 | 🟠 | Đồng bộ 9 commit mới của upstream (`24d7968..a8de813`, có skill `offensive-netexec` và bản sửa repoint action của tessl) | Fork đang chậm upstream 9 commit | `git log HEAD..upstream/main` rỗng |
| 3 | 🟢 | Ghim 10 action còn lại theo SHA và thêm `permissions:` cấp đầu cho `codeql.yml`, `skill-optimize-apply.yml`, `skill-preview.yml` | Ghim 2/12 | Grep `uses:` không ghim trả về 0 |

## Giữ nguyên (điểm mạnh)

- SECURITY.md ghi rõ mục đích sử dụng; manifest skill. Pack dual-use: chỉ dùng khi được ủy quyền và chạy trong sandbox.

## Gợi ý tích hợp từ catalog `stored`

- Quét bằng SkillSpector, cisco skill-scanner hoặc agent-scan trước khi cài (stored mục 12 §3e); Decepticon là bản "chạy được" cùng mục đích (§3g); template out-brief của AI Red Teaming Guide (§3f).

## Ghi chú chung

- Ghim action theo SHA bằng [pinact](https://github.com/suzuki-shunsuke/pinact) hoặc [ratchet](https://github.com/sethvargo/ratchet), giữ comment tag (`# v7.0.1`), rồi để Dependabot `github-actions` tự nâng SHA. Đây là cách projects-template, xboss và Sales-Hunter đang làm.
- Chuẩn tham chiếu là `seeker19110/projects-template`: 25/25 action ghim SHA, mọi workflow có `permissions:` cấp đầu, Dependabot cho npm, github-actions và pip.
