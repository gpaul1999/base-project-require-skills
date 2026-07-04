# Skills Catalog

Danh mục skill cho AI agent. Hai tầng: **Vendored skills** (viết trực tiếp ở đây) và
**External toolkits** (bộ lớn, chỉ tham chiếu + hướng dẫn cài).

---

## 0. Agent chọn skill nào & cài ra sao

Có **2 cơ chế kích hoạt**:
- **Vendored skills** → **agent TỰ chọn** theo `description` (miễn là file skill nằm trong skill-path của project).
- **External toolkits** → **KHÔNG auto**. Agent chỉ dùng nếu (a) đã **cài** từ nguồn, và (b) có **luật routing**
  trong `CLAUDE.md`/`AGENTS.md` của project (bảng dưới).

### Bảng routing — "khi user muốn … → dùng …"

| Ý định của user | Dùng | Loại | Cách kích hoạt |
|---|---|---|---|
| Chạy E2E test / automation browser / chụp screenshot | `playwright-e2e` | vendored | **Auto** theo description |
| Đọc nội dung file PDF/Word/Excel/PPT/ảnh/audio… | `markitdown` | vendored | **Auto** theo description |
| Chốt *"build gì & vì sao"*, product judgment, ship nhanh (Claude Code) | `gstack` | external | Manual: `/office-hours`, `/plan-*`, `/ship`, `/retro` |
| Triển khai feature có kỷ luật, spec→code (kể cả **Codex**) | `spec-kit` | external | Manual: `/speckit.specify → plan → tasks → implement` |
| Làm UI landing/portfolio/redesign "có gu" | `taste-skill` | external | Auto (nếu copy vào `.claude/skills/`) hoặc manual |
| Việc marketing/growth (CRO, copy, SEO, ads…) | `marketingskills` | external | Manual: `/cro`, `/copywriting`, `/seo`… |
| Biến nội dung/dữ liệu → HTML/deck/social-card | `html-anything` | tool | Chạy app riêng (localhost) |

> Chồng lấn `plan→tasks`: gstack vs spec-kit — xem mục cuối. UI: `taste-skill` (code UI có gu) vs
> `html-anything` (app sinh HTML từ nội dung), khác nhau.

### Bảng cài đặt

| Skill | Cài |
|---|---|
| `playwright-e2e` | `claude mcp add playwright npx @playwright/mcp@latest` |
| `markitdown` | `pip install markitdown-mcp` + `claude mcp add markitdown markitdown-mcp` |
| `gstack` | `git clone …/garrytan/gstack ~/.claude/skills/gstack && ./setup` |
| `spec-kit` | `uv tool install specify-cli --from git+https://github.com/github/spec-kit.git` |
| `taste-skill` | `npx skills add https://github.com/leonxlnx/taste-skill` |
| `marketingskills` | `npx skills add coreyhaines31/marketingskills` |
| `html-anything` | `git clone …/nexu-io/html-anything && pnpm i && pnpm -F @html-anything/next dev` |

*(Lệnh chi tiết + lưu ý ở từng mục bên dưới.)*

### Wire vào một project mới

Để project mới "refer" tới repo này và agent route đúng:

1. **Đưa vendored skills vào tầm với của agent** (chọn 1):
   - Submodule: `git submodule add <repo-url> .agent-skills` rồi symlink/copy `skills/playwright-e2e`,
     `skills/markitdown` vào `.claude/skills/`.
   - Hoặc copy trực tiếp 2 thư mục skill đó vào `.claude/skills/`.
2. **Cài external toolkit** bạn cần theo bảng cài đặt (chỉ cài cái dự án thực sự dùng — *Simplicity First*).
3. **Dán đoạn sau vào `CLAUDE.md` (Claude Code) hoặc `AGENTS.md` (Codex) của project** để agent biết routing:

   ```markdown
   ## Skills & toolkits
   Tuân theo bộ quy tắc & bảng routing tại <repo-url>/skills/README.md.
   - E2E test / browser / screenshot → skill `playwright-e2e` (auto).
   - Đọc file PDF/Office/ảnh/audio → skill `markitdown` (auto).
   - Product planning / ship / retro → toolkit `gstack` (nếu đã cài): /office-hours, /plan-*, /ship, /retro.
   - Spec-driven implement (kể cả Codex) → `spec-kit`: /speckit.*.
   - UI landing/portfolio có gu → `taste-skill`. Marketing/growth → `marketingskills` (/cro, /copywriting…).
   - Content → HTML/deck → chạy `html-anything`.
   Chỉ dùng toolkit đã được cài; nếu chưa cài mà cần, báo user cài trước.
   ```

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
| [`playwright-e2e`](playwright-e2e/SKILL.md) | E2E testing & browser automation qua Playwright MCP, có chụp screenshot làm bằng chứng | Test / QA |
| [`markitdown`](markitdown/SKILL.md) | Chuyển PDF/Word/Excel/PPT/ảnh/audio/HTML… thành Markdown (MarkItDown MCP) để agent đọc được | Đọc tài liệu |

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

### spec-kit — Spec-Driven Development

- **Nguồn**: https://github.com/github/spec-kit (GitHub) · Open-source
- **Là gì**: Toolkit cho **Spec-Driven Development** — viết spec trước, rồi sinh plan → tasks →
  implement một cách có hệ thống và **truy vết được**. Agent-agnostic: hỗ trợ **30+ AI agent**
  (Claude Code, Codex/Copilot, Cursor, Gemini…).
- **Dùng ở phase nào**: **Triển khai feature có kỷ luật** — khi cần spec rõ ràng, artifact bền vững,
  hoặc khi làm với **Codex/agent khác** (gstack chỉ chạy Claude Code).
- **Command tiêu biểu**: `/speckit.constitution` → `/speckit.specify` → `/speckit.plan` →
  `/speckit.tasks` → `/speckit.implement` → `/speckit.converge` (phụ: `/speckit.clarify`,
  `/speckit.analyze`, `/speckit.checklist`).
- **Artifact sinh ra**: `constitution.md`, `spec.md`, `plan.md`, `tasks.md`.
- **Cài** (cần `uv` + Python 3.11+):
  ```bash
  uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
  specify init my-project        # thêm --ai claude / --ai codex tuỳ agent; xem `specify integration list`
  ```

### taste-skill — UI "có gu" (anti-slop frontend)

- **Nguồn**: https://github.com/leonxlnx/taste-skill · License **MIT**
- **Là gì**: Toolkit design giúp agent tạo **UI có gu, không bị templated/slop** — layout, typography,
  motion, spacing mạnh hơn. Skill chính `design-taste-frontend` self-contained; có nhiều **biến thể**:
  `soft`, `minimalist`, `brutalist`, `gpt-taste`, `imagegen-*`, `brandkit`.
- **Dùng khi nào**: **lúc thiết kế/triển khai UI** — landing page, portfolio, redesign; muốn UI trông
  chủ đích và tinh tế hơn. Có 3 "dial" chỉnh: Design Variance, Motion Intensity, Visual Density.
- **⚠ Scope giới hạn**: chỉ landing/portfolio/redesign. **KHÔNG** hợp cho dashboard, data table,
  mobile native, realtime collab UI — đừng gọi skill này cho mấy loại đó.
- **Cài / dùng**:
  ```bash
  npx skills add https://github.com/leonxlnx/taste-skill     # Vercel Agent Skills CLI
  ```
  Hoặc copy thẳng thư mục skill mong muốn vào `.claude/skills/` (mỗi SKILL.md là markdown standalone).
- **Lưu ý**: skill rất dài (~15k từ) và đang được maintain (v2 experimental) → **tham chiếu/cài từ nguồn**,
  không copy verbatim vào repo này để tránh drift.

### marketingskills — bộ skill marketing (CRO, copy, SEO, growth)

- **Nguồn**: https://github.com/coreyhaines31/marketingskills · License **MIT** · ~36k★ (tác giả Corey Haines)
- **Là gì**: **60+ skill marketing** — CRO/onboarding/paywall, copywriting/cold-email/social, SEO/AI-search/
  programmatic SEO, ads, analytics/A-B test, churn, pricing, launch, RevOps… Mọi skill đọc chung file nền
  `product-marketing.md` trước khi chạy.
- **Dùng khi nào**: cần agent làm **việc marketing/growth** — tối ưu landing, viết copy/email, audit SEO,
  lên plan launch, pricing… (gọi trực tiếp "optimize this landing page" hoặc `/cro`, `/copywriting`).
- **Cài / dùng**:
  ```bash
  npx skills add coreyhaines31/marketingskills     # hoặc dùng qua Claude Code plugin / git submodule
  ```
- **Đã cân nhắc phương án khác**: `alirezarezvani/claude-skills` (19.9k★ nhưng là kho tạp 18 domain, marketing
  chỉ 48/354 skill — không curate riêng), `kostja94/marketing-skills` (701★, breadth lớn nhưng ít validate).
  → coreyhaines31 là bản **thuần marketing, adoption cao nhất, tác giả domain-expert** ⇒ chọn.

### html-anything — content → HTML đẹp (tool, không phải skill)

- **Nguồn**: https://github.com/nexu-io/html-anything · License **Apache-2.0** · 7.5k★ (team Open Design)
- **Là gì**: **Web app local-first** ("agentic HTML editor") — gọi CLI agent có sẵn (`claude`, `codex`…) để
  biến **Markdown/CSV/JSON/SQL/text → HTML single-file** đẹp, ship-ready. **Không phải SKILL.md/skill library** —
  là công cụ để *chạy*, không auto-load như các skill khác.
- **Dùng khi nào**: cần biến nội dung/dữ liệu thành **HTML/deck/social-card/office-doc** trau chuốt nhanh
  (75 template qua 9 surface, export HTML/PNG).
- **⚠ Lưu ý**: design constraint **CJK-first** (font Trung/Nhật/Hàn), export nhắm nhiều nền tảng social TQ
  (WeChat/Weibo/Xiaohongshu/Zhihu) → tối ưu cho ngữ cảnh CJK.
- **Chạy**:
  ```bash
  git clone https://github.com/nexu-io/html-anything && cd html-anything
  pnpm install && pnpm -F @html-anything/next dev      # → http://localhost:3000
  ```
- **Phân biệt với `taste-skill`**: `taste-skill` = *code UI trong project cho có gu*; `html-anything` =
  *app riêng để sinh HTML/deck từ nội dung*. Bổ trợ nhau, không thay thế.

### gstack vs spec-kit — chọn cái nào?

Không phải chọn 1 — hai bộ khác trọng tâm & khác agent. Chồng lấn chỉ ở khúc `plan → tasks`;
**đừng chạy cả hai pipeline plan cùng lúc trên cùng một feature**.

| Nhu cầu | Dùng |
|---|---|
| Chốt *"nên build gì & vì sao"*, product judgment, ship nhanh (Claude Code) | **gstack** (`/office-hours` …) |
| Triển khai có kỷ luật, spec/artifact bền vững, truy vết | **spec-kit** |
| Đang dùng **Codex / agent không phải Claude Code** | **spec-kit** (gstack không hỗ trợ) |
| Feature lớn, muốn kết hợp | gstack `/office-hours` → spec-kit `/speckit.specify → … → implement` |
