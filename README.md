# AI Agent Skills — Base Project

Kho tổng hợp **quy tắc chung + skill cần thiết** cho AI coding agent (Claude Code, Codex, …).
Mục tiêu: một project mới chỉ cần *refer* tới repo này là có ngay bộ quy tắc và skill để bắt đầu.

## Import bằng 1 lệnh

Trong project bất kỳ, ra lệnh cho agent (ví dụ):

> "Import skills từ `<repo-url>` — đọc `skills/import-skills/SKILL.md` và làm theo."

Agent sẽ đọc skill [`import-skills`](skills/import-skills/SKILL.md), hỏi bạn chọn skill cần dùng, cài chúng
(vendored + MCP server, external toolkit), rồi ghi routing block vào `CLAUDE.md`/`AGENTS.md` của project.
Không cần bạn làm thủ công từng bước.

## Cấu trúc

```
CLAUDE.md      # Quy tắc hành vi chung cho agent (think-before-coding, simplicity, surgical changes…)
DEVLOG.md      # Nhật ký thay đổi của chính repo này
README.md      # File này
skills/        # Mục skill — xem skills/README.md để biết cách tổ chức & danh mục
```

## Mục skill có 2 tầng

1. **Vendored skills** — skill nhỏ, standalone. Viết trực tiếp dạng `skills/<ten>/SKILL.md`
   (chuẩn Claude Code, có frontmatter `name` + `description`). Claude Code tự nạp; Codex đọc như markdown.
2. **External toolkits** — bộ skill lớn, có installer & phụ thuộc riêng (vd: gstack). **Không vendor** —
   chỉ tham chiếu trong catalog kèm hướng dẫn cài, để không drift so với upstream.

Danh mục đầy đủ: [`skills/README.md`](skills/README.md).

## Project mới "refer" tới repo này thế nào

### Claude Code
- **Vendored skills**: đóng gói repo thành plugin và `/plugin install` (thêm khi đã có skill vendored đầu tiên),
  hoặc clone repo vào `.claude/skills/`.
- **External toolkits**: cài theo lệnh ghi trong catalog (thường cài global vào `~/.claude/skills/`).

### Codex / agent khác
- Thêm repo này làm **git submodule**, rồi trong `AGENTS.md`/`CLAUDE.md` của project trỏ tới
  `CLAUDE.md` và thư mục `skills/` ở đây.

### Agent chọn skill nào & cài đặt
Xem [`skills/README.md` §0](skills/README.md#0-agent-chọn-skill-nào--cài-ra-sao): bảng **routing**
(ý định → skill), bảng **cài đặt**, và đoạn **wiring** dán sẵn vào `CLAUDE.md`/`AGENTS.md` của project mới.
