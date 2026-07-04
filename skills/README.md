# Skills Catalog

Danh mục skill cho AI agent. Hai tầng: **Vendored skills** (viết trực tiếp ở đây) và
**External toolkits** (bộ lớn, chỉ tham chiếu + hướng dẫn cài).

---

## Vendored skills

Mỗi skill là 1 thư mục `skills/<ten>/SKILL.md` với frontmatter tối thiểu:

```yaml
---
name: ten-skill
description: Mô tả NGẮN, rõ, có trigger — Claude Code dựa vào đây để quyết định khi nào kích hoạt skill.
---
```

Nội dung thân skill: cô đọng, actionable (đúng tinh thần *Simplicity First* trong `CLAUDE.md`).

| Skill | Mô tả | Phase |
|-------|-------|-------|
| _(chưa có)_ | | |

---

## External toolkits (tham chiếu, không vendor)

Bộ skill lớn có installer/phụ thuộc riêng. Project cài thẳng từ nguồn để luôn cập nhật.

### gstack — AI Software Factory

- **Nguồn**: https://github.com/garrytan/gstack · License **MIT**
- **Là gì**: Toolkit biến Claude Code thành "virtual engineering team" — **23 skills + 8 power tools**
  phủ trọn vòng đời sản phẩm: **think → plan → build → review → test → ship → reflect**.
- **Dùng ở phase nào**: **Xây dựng tính năng sản phẩm** (planning, review, QA, ship, retro) —
  KHÔNG phải coding thuần. Gọi khi bắt đầu một feature/sprint, cần challenge ý tưởng, review kiến trúc,
  hoặc chuẩn hoá quy trình ship & retro.
- **Skill/command tiêu biểu**:
  - Think: `/office-hours` (challenge giả định sản phẩm bằng câu hỏi forcing)
  - Plan: `/plan-ceo-review`, `/plan-eng-review`, `/plan-design-review`
  - Review/QA: `/review`, `/codex`, `/qa`, `/cso` (security)
  - Ship: `/ship`, `/land-and-deploy`, `/document-release`
  - Reflect: `/retro`, `/learn`
  - Safety/automation: `/careful`, `/freeze`, `/guard`, `/autoplan`, `/browse`
- **Cài (global)**:
  ```bash
  git clone --single-branch --depth 1 https://github.com/garrytan/gstack.git ~/.claude/skills/gstack \
    && cd ~/.claude/skills/gstack && ./setup
  ```
  Chia sẻ trong team: `./setup --team` (bootstrap project, không vendor file, không drift version).
- **Lưu ý**: mỗi `SKILL.md` của gstack phụ thuộc `lib/`, `scripts/`, section-files nội bộ →
  **không copy lẻ**; cài nguyên toolkit như trên.
