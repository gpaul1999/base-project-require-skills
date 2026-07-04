# DEVLOG

Nhật ký thay đổi. 1 dòng / task. Entry > 7 ngày tự dọn; > 30 dòng thì nén thành dòng tóm tắt.

- 2026-07-04: Thêm vendored skill `import-skills` — agent tự đọc & thực thi bootstrap (cài skill đã chọn + ghi routing block vào CLAUDE.md/AGENTS.md); root README thêm mục "Import bằng 1 lệnh".
- 2026-07-04: Thêm §0 "Agent chọn skill nào & cài ra sao" — bảng routing (ý định→skill), bảng cài đặt, đoạn wiring dán vào CLAUDE.md/AGENTS.md; giải thích 2 cơ chế auto (vendored) vs manual (external).
- 2026-07-04: Thêm tool `html-anything` (nexu-io, Apache-2.0) — content→HTML đẹp; ghi rõ là tool để chạy + CJK-first, phân biệt với taste-skill.
- 2026-07-04: Thêm external toolkit `marketingskills` (coreyhaines31, ~36k★) sau khi so sánh với alirezarezvani/claude-skills & kostja94 — chọn bản thuần marketing, adoption cao nhất.
- 2026-07-04: Thêm external toolkit `taste-skill` (UI anti-slop, làm UI "có gu") — tham chiếu, ghi rõ scope chỉ landing/portfolio/redesign.
- 2026-07-04: Thêm external toolkit `spec-kit` (Spec-Driven Dev, agent-agnostic) + bảng so sánh gstack vs spec-kit (giữ cả hai, khác trọng tâm & agent).
- 2026-07-04: Thêm vendored skill `markitdown` (chuyển tài liệu PDF/Office/ảnh/audio… sang Markdown qua MarkItDown MCP); cập nhật catalog.
- 2026-07-04: Thêm vendored skill `playwright-e2e` (E2E testing + browser automation qua Playwright MCP, chụp screenshot); cập nhật catalog.
- 2026-07-04: Dựng khung mục skill (README, skills/README catalog 2 tầng, DEVLOG). Thêm gstack vào tầng External toolkits (tham chiếu, không vendor) — phase xây dựng tính năng sản phẩm.
