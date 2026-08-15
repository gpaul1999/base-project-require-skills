# DEVLOG

Nhật ký thay đổi. 1 dòng / task. Entry > 7 ngày tự dọn; > 30 dòng thì nén thành dòng tóm tắt.

- 2026-08-15: Đính chính 2 điểm ở mục `mattpocock` — (a) nhóm ✅ **vendor lẻ được** (7 skill tự chứa, `CONTEXT.md` là của project đích chứ không phải repo gốc), (b) skill `code-review` **trùng tên built-in Claude Code** nên bị che, phải đổi tên mới dùng được. Ghi giải pháp tạm khi không fork được: vendor snapshot MIT tại `8b78b53`.
- 2026-08-15: Verify supply-chain pin — gstack/taste-skill/marketingskills: HEAD khớp đúng commit đã pin ✅. `gpaul1999/skills` (mattpocock) **không truy cập được** (404 anonymous) → đánh dấu ⏸ ở catalog, ghi việc cần làm (fork lại + public + pin SHA mới).
- 2026-08-15: Ghi chú `harness` (revfactory, orchestration Layer 3) như mục nâng cao tuỳ chọn — không xung đột cứng gstack (khác tầng) nhưng chồng khái niệm "team"; chưa vào core, chưa fork/route.
- 2026-08-15: Thêm external toolkit `mattpocock` (fork gpaul1999/skills @ 8b78b53) — lọc chỉ nhóm coding-discipline (tdd, diagnosing-bugs, code-review, domain-modeling, codebase-design, research, resolving-merge-conflicts); routing tránh nhóm spec-pipeline trùng spec-kit. Đồng thời vá pin marketingskills v2.6.0→30dbd7f (bản sửa bị sót khỏi PR#1 đã merge).
- 2026-07-04: Repoint toàn bộ lệnh cài sang fork gpaul1999/* + pin cụ thể (gstack@11de390, taste-skill@06d6028, marketingskills@30dbd7f, playwright@0.0.77, markitdown==0.0.1a4). Review cuối: catalog nhất quán, sẵn sàng v1.0.
- 2026-07-04: Thêm §0 "Supply-chain: fork + pin" — bảng pin (marketingskills v2.6.0; gstack/taste-skill pin commit SHA; playwright/markitdown pin version) + lệnh fork; import-skills ưu tiên nguồn fork+pin.
- 2026-07-04: Thêm vendored skill `import-skills` — agent tự đọc & thực thi bootstrap (cài skill đã chọn + ghi routing block vào CLAUDE.md/AGENTS.md); root README thêm mục "Import bằng 1 lệnh".
- 2026-07-04: Thêm §0 "Agent chọn skill nào & cài ra sao" — bảng routing (ý định→skill), bảng cài đặt, đoạn wiring dán vào CLAUDE.md/AGENTS.md; giải thích 2 cơ chế auto (vendored) vs manual (external).
- 2026-07-04: Thêm tool `html-anything` (nexu-io, Apache-2.0) — content→HTML đẹp; ghi rõ là tool để chạy + CJK-first, phân biệt với taste-skill.
- 2026-07-04: Thêm external toolkit `marketingskills` (coreyhaines31, ~36k★) sau khi so sánh với alirezarezvani/claude-skills & kostja94 — chọn bản thuần marketing, adoption cao nhất.
- 2026-07-04: Thêm external toolkit `taste-skill` (UI anti-slop, làm UI "có gu") — tham chiếu, ghi rõ scope chỉ landing/portfolio/redesign.
- 2026-07-04: Thêm external toolkit `spec-kit` (Spec-Driven Dev, agent-agnostic) + bảng so sánh gstack vs spec-kit (giữ cả hai, khác trọng tâm & agent).
- 2026-07-04: Thêm vendored skill `markitdown` (chuyển tài liệu PDF/Office/ảnh/audio… sang Markdown qua MarkItDown MCP); cập nhật catalog.
- 2026-07-04: Thêm vendored skill `playwright-e2e` (E2E testing + browser automation qua Playwright MCP, chụp screenshot); cập nhật catalog.
- 2026-07-04: Dựng khung mục skill (README, skills/README catalog 2 tầng, DEVLOG). Thêm gstack vào tầng External toolkits (tham chiếu, không vendor) — phase xây dựng tính năng sản phẩm.
