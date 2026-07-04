---
name: import-skills
description: Bootstrap this base repo's agent skills into the CURRENT project — pick the needed skills, install them (vendored skills + their MCP servers, and external toolkits), then write the routing block into the project's CLAUDE.md / AGENTS.md. Use when the user says "import skills from the base repo", "bootstrap agent skills", "set up skills from <repo>", or similar.
---

# Import skills from the base skills repo

Mục tiêu: wire catalog skill của repo này vào **project mà user đang làm việc**, để agent
tự dùng đúng skill về sau. Chạy khi user ra lệnh kiểu "import skills từ repo <url>".

## Các bước (agent tự thực thi)

1. **Đọc nguồn chân lý.** Mở `skills/README.md` của repo này — **§0** có bảng *routing* + bảng *cài đặt*
   authoritative và ghi chú từng skill. **Không** dùng lệnh/version từ trí nhớ; đọc từ đây để không lệch.
   (Nếu chưa có repo trên máy: clone nó, hoặc đọc raw file trên GitHub.)

2. **Nhận diện agent & project đích.**
   - Claude Code → project dùng `CLAUDE.md` + `.claude/skills/`.
   - Codex → `AGENTS.md`.
   - Ghi nhận tín hiệu stack (có app browser? cần đọc tài liệu? làm UI? marketing?) để gợi ý skill phù hợp.

3. **Hỏi user chọn skill nào để import** (dùng `AskUserQuestion`, multiSelect) — liệt kê 7 skill kèm 1 dòng
   công dụng, tick sẵn cái khớp stack. **Chỉ import cái được chọn** (Simplicity First).

4. **Cài từng skill đã chọn** theo bảng cài đặt §0. **Hỏi xác nhận trước khi chạy** bất kỳ lệnh
   cài/mạng nào.
   - Vendored (`playwright-e2e`, `markitdown`): copy thư mục skill vào `.claude/skills/` **và** chạy lệnh
     MCP của nó (`claude mcp add …`, `pip install …`).
   - External toolkit: chạy lệnh cài tương ứng.

5. **Ghi routing block** vào `CLAUDE.md` (Claude Code) hoặc `AGENTS.md` (Codex) của project — chỉ các dòng
   cho skill đã chọn. Có sẵn mục "## Skills & toolkits" thì cập nhật; chưa có thì thêm mới. Mẫu block ở §0.

6. **Verify & báo cáo.** `claude mcp list` để xác nhận MCP đã thêm; liệt kê skill đã copy; xác nhận routing
   block. Báo user những gì đã cài + việc cần tự hoàn tất (vd `html-anything` phải `pnpm dev`).

## Rules
- Chỉ đụng vào skill user đã chọn; không cài dư.
- Không chạy lệnh cài/mạng khi chưa được user xác nhận.
- Lệnh cài đọc từ `skills/README.md §0`, không hardcode từ trí nhớ → luôn đồng bộ khi catalog đổi.
