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
| Viết test trước / TDD unit | `mattpocock` → `tdd` | external | Manual |
| Debug bug khó, bài bản (red→fix) | `mattpocock` → `diagnosing-bugs` | external | Manual |
| Review code (2 trục: Standards + Spec) | `mattpocock` → `code-review` | external | Manual |
| Thiết kế module sâu / kiến trúc / domain model | `mattpocock` → `domain-modeling`, `codebase-design` | external | Manual |
| Điều tra câu hỏi, có trích dẫn nguồn | `mattpocock` → `research` | external | Manual |
| Xử lý git merge conflict theo intent | `mattpocock` → `resolving-merge-conflicts` | external | Manual |

> Chồng lấn `plan→tasks`: gstack vs spec-kit — xem mục cuối. UI: `taste-skill` (code UI có gu) vs
> `html-anything` (app sinh HTML từ nội dung), khác nhau.
> **mattpocock** lấp tầng *coding discipline* (TDD/debug/review/design). **Tránh** dùng nhóm spec-pipeline
> của nó (`to-spec`, `to-tickets`, `wayfinder`, `implement`, `triage`) — đã có **spec-kit** lo, để không thừa.

### Bảng cài đặt

Nguồn đã **fork về `gpaul1999/*` + pin** (xem mục Supply-chain). Package MS pin version.

| Skill | Cài |
|---|---|
| `playwright-e2e` | `claude mcp add playwright npx @playwright/mcp@0.0.77` |
| `markitdown` | `pip install markitdown-mcp==0.0.1a4` + `claude mcp add markitdown markitdown-mcp` |
| `gstack` | `git clone …/gpaul1999/gstack ~/.claude/skills/gstack && ./setup` |
| `spec-kit` | `uv tool install specify-cli --from git+https://github.com/github/spec-kit.git` |
| `taste-skill` | `npx skills add https://github.com/gpaul1999/taste-skill` |
| `marketingskills` | `npx skills add gpaul1999/marketingskills` |
| `html-anything` | `git clone …/nexu-io/html-anything && pnpm i && pnpm -F @html-anything/next dev` |
| `mattpocock` | `npx skills add https://github.com/gpaul1999/skills` |

*(Lệnh chi tiết + lưu ý ở từng mục bên dưới. Yêu cầu: đã fork sang `gpaul1999` — xem Supply-chain.)*

### Supply-chain: fork + pin (chống repo biến mất / đổi bậy)

External toolkit là repo bên thứ ba → có thể **chuyển private, bị xoá, đổi license, hoặc push update
hỏng/độc hại**. Phòng vệ:

- **License MIT/Apache-2.0 không thu hồi được** với bản đã fork → **fork ngay** là khoá quyền dùng bản đó vĩnh viễn.
- **Fork thôi chưa đủ — phải PIN** 1 tag/commit đã review, vì fork sync theo upstream vẫn có thể kéo về bản xấu.
  `npx skills add @latest` / `git clone` HEAD = nuốt bất cứ thứ gì upstream đang có.

Đã fork về `gpaul1999/*` và pin mốc dưới đây. **Fork chưa sync = snapshot đóng băng tại mốc pin** →
tự động reproducible, không lệ thuộc upstream.

| Toolkit | Upstream | Fork (đang dùng) | Mốc pin |
|---|---|---|---|
| `marketingskills` | `coreyhaines31/marketingskills` | `gpaul1999/marketingskills` | commit **`30dbd7f`** (fork chỉ copy main, không kèm tag `v2.6.0`) |
| `gstack` | `garrytan/gstack` | `gpaul1999/gstack` | commit **`11de390`** |
| `taste-skill` | `leonxlnx/taste-skill` | `gpaul1999/taste-skill` | commit **`06d6028`** |
| `mattpocock` | `mattpocock/skills` | `gpaul1999/skills` | commit **`8b78b53`** (mới hơn tag `v1.2.3` vài commit) |
| `spec-kit` | `github/spec-kit` | *(chưa fork — org lớn, tuỳ chọn)* | tag release |
| `html-anything` | `nexu-io/html-anything` | *(chưa fork — org, tuỳ chọn)* | tag/commit |
| `playwright-e2e` | `@playwright/mcp` (npm) | — không cần fork | pin **`@0.0.77`** |
| `markitdown` | `markitdown-mcp` (pip) | — không cần fork | pin **`==0.0.1a4`** |

**Lệnh fork (chạy 1 lần — bắt buộc trước khi lệnh cài ở trên hoạt động):**
```bash
gh repo fork coreyhaines31/marketingskills --clone=false   # gpaul1999/marketingskills @ 30dbd7f
gh repo fork garrytan/gstack               --clone=false   # gpaul1999/gstack          @ 11de390
gh repo fork leonxlnx/taste-skill          --clone=false   # gpaul1999/taste-skill     @ 06d6028
gh repo fork mattpocock/skills             --clone=false   # gpaul1999/skills          @ 8b78b53
```
Mốc pin (đã verify — mỗi fork đóng băng tại main HEAD của nó; GitHub fork không copy tag nên pin theo commit):
`gstack=11de390be1be6849eb9a15f91ff4922dd16c589a`, `taste-skill=06d6028b5c623016c59ce8536f578e5a1127b499`,
`marketingskills=30dbd7f793b86f0ec2f007757b333afac93c24db` (mới hơn tag `v2.6.0`),
`mattpocock=8b78b531ab965735c5dc74f6f7a219e1e37326df` (mới hơn tag `v1.2.3`). Muốn đúng release thì push tag sang fork thủ công.

**Quan trọng — đừng bấm "Sync fork"** cho tới khi bạn đã review & muốn cập nhật; giữ fork nguyên = giữ pin.
Muốn cứng hơn nữa: sau fork, tạo branch/tag đóng băng đúng SHA rồi cài từ đó. Review nội dung skill (gstack,
taste-skill là owner cá nhân) trước khi tin dùng — skill là chỉ thị agent sẽ chạy.

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
   - Coding discipline (TDD, debug, review code, thiết kế module, research, merge conflict) → `mattpocock`
     (nhóm: tdd, diagnosing-bugs, code-review, domain-modeling, codebase-design, research, resolving-merge-conflicts).
     TRÁNH nhóm spec-pipeline của mattpocock (to-spec/to-tickets/implement…) — đã có spec-kit.
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
| [`import-skills`](import-skills/SKILL.md) | Bootstrap catalog này vào project hiện tại: cài skill đã chọn + ghi routing block | Setup |
| [`playwright-e2e`](playwright-e2e/SKILL.md) | E2E testing & browser automation qua Playwright MCP, có chụp screenshot làm bằng chứng | Test / QA |
| [`markitdown`](markitdown/SKILL.md) | Chuyển PDF/Word/Excel/PPT/ảnh/audio/HTML… thành Markdown (MarkItDown MCP) để agent đọc được | Đọc tài liệu |

---

## External toolkits (tham chiếu, không vendor)

Bộ skill lớn có installer/phụ thuộc riêng. Project cài thẳng từ nguồn để luôn cập nhật.

### gstack — AI Software Factory

- **Nguồn**: fork `https://github.com/gpaul1999/gstack` @ `11de390` (upstream `garrytan/gstack`) · License **MIT**
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
- **Cài (global, từ fork đã pin)**:
  ```bash
  git clone --single-branch --depth 1 https://github.com/gpaul1999/gstack.git ~/.claude/skills/gstack \
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

- **Nguồn**: fork `https://github.com/gpaul1999/taste-skill` @ `06d6028` (upstream `leonxlnx/taste-skill`) · License **MIT**
- **Là gì**: Toolkit design giúp agent tạo **UI có gu, không bị templated/slop** — layout, typography,
  motion, spacing mạnh hơn. Skill chính `design-taste-frontend` self-contained; có nhiều **biến thể**:
  `soft`, `minimalist`, `brutalist`, `gpt-taste`, `imagegen-*`, `brandkit`.
- **Dùng khi nào**: **lúc thiết kế/triển khai UI** — landing page, portfolio, redesign; muốn UI trông
  chủ đích và tinh tế hơn. Có 3 "dial" chỉnh: Design Variance, Motion Intensity, Visual Density.
- **⚠ Scope giới hạn**: chỉ landing/portfolio/redesign. **KHÔNG** hợp cho dashboard, data table,
  mobile native, realtime collab UI — đừng gọi skill này cho mấy loại đó.
- **Cài / dùng** (từ fork đã pin):
  ```bash
  npx skills add https://github.com/gpaul1999/taste-skill     # Vercel Agent Skills CLI
  ```
  Hoặc copy thẳng thư mục skill mong muốn vào `.claude/skills/` (mỗi SKILL.md là markdown standalone).
- **Lưu ý**: skill rất dài (~15k từ) và đang được maintain (v2 experimental) → **tham chiếu/cài từ nguồn**,
  không copy verbatim vào repo này để tránh drift.

### marketingskills — bộ skill marketing (CRO, copy, SEO, growth)

- **Nguồn**: fork `https://github.com/gpaul1999/marketingskills` @ `30dbd7f` (upstream `coreyhaines31/marketingskills`, ~36k★) · License **MIT**
- **Là gì**: **60+ skill marketing** — CRO/onboarding/paywall, copywriting/cold-email/social, SEO/AI-search/
  programmatic SEO, ads, analytics/A-B test, churn, pricing, launch, RevOps… Mọi skill đọc chung file nền
  `product-marketing.md` trước khi chạy.
- **Dùng khi nào**: cần agent làm **việc marketing/growth** — tối ưu landing, viết copy/email, audit SEO,
  lên plan launch, pricing… (gọi trực tiếp "optimize this landing page" hoặc `/cro`, `/copywriting`).
- **Cài / dùng** (từ fork đã pin `30dbd7f`):
  ```bash
  npx skills add gpaul1999/marketingskills     # kéo main của fork = 30dbd7f (đã đóng băng)
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

### mattpocock — kỷ luật engineering (tầng coding)

- **Nguồn**: fork `https://github.com/gpaul1999/skills` @ `8b78b53` (upstream `mattpocock/skills`, Matt Pocock) · License **MIT**
- **Là gì**: Bộ ~24 skill về **kỷ luật engineering khi code với AI**. Lấp đúng tầng *coding discipline* mà bộ
  hiện tại thiếu (gstack lo product, spec-kit lo spec→plan).
- **✅ Chỉ dùng nhóm lấp gap** (đã lọc để không thừa):
  - `tdd` — red→green→refactor, viết test trước.
  - `diagnosing-bugs` — vòng lặp debug bài bản cho bug khó.
  - `code-review` — review 2 trục (Standards + Spec) qua sub-agent song song.
  - `domain-modeling` + `codebase-design` — dựng domain model, thiết kế module sâu / interface nhỏ.
  - `research` — điều tra câu hỏi theo primary source, xuất Markdown có trích dẫn.
  - `resolving-merge-conflicts` — gỡ git conflict theo intent, từng hunk.
- **⛔ TRÁNH nhóm trùng** (đã có tool khác lo, dùng sẽ thừa/chồng chéo):
  `to-spec`, `to-tickets`, `wayfinder`, `implement`, `triage`, `setup-*` → **spec-kit** đã lo pipeline spec→plan→tasks→implement.
  `prototype` → đã có `taste-skill`/`html-anything`.
- **Cài / dùng** (từ fork đã pin):
  ```bash
  npx skills add https://github.com/gpaul1999/skills     # cài cả bộ; chỉ dùng nhóm ✅ theo routing ở §0
  ```
- **Lưu ý**: skill trong bộ **phụ thuộc lẫn nhau** (primitive `grilling`, file `CONTEXT.md`, skill `setup-*`)
  → cài cả bộ, đừng vendor lẻ; việc "không thừa" xử lý bằng **routing** (chỉ gọi nhóm ✅), không phải xoá file.

### gstack vs spec-kit — chọn cái nào?

Không phải chọn 1 — hai bộ khác trọng tâm & khác agent. Chồng lấn chỉ ở khúc `plan → tasks`;
**đừng chạy cả hai pipeline plan cùng lúc trên cùng một feature**.

| Nhu cầu | Dùng |
|---|---|
| Chốt *"nên build gì & vì sao"*, product judgment, ship nhanh (Claude Code) | **gstack** (`/office-hours` …) |
| Triển khai có kỷ luật, spec/artifact bền vững, truy vết | **spec-kit** |
| Đang dùng **Codex / agent không phải Claude Code** | **spec-kit** (gstack không hỗ trợ) |
| Feature lớn, muốn kết hợp | gstack `/office-hours` → spec-kit `/speckit.specify → … → implement` |

---

## Orchestration nâng cao (tuỳ chọn — CHƯA thuộc core)

**`harness`** (`revfactory/harness`, Apache-2.0, ~8.8k★) — *meta-skill Layer 3*: phân tích domain →
thiết kế "agent team" → sinh agent/skill vào `.claude/agents/` & `.claude/skills/`, theo 6 pattern
(pipeline, fan-out, supervisor, producer-reviewer, expert-pool, hierarchical).

- **Vị trí**: ngồi **TRÊN** các toolkit thực thi. `harness` lo *"ai phối hợp & thế nào"*; gstack/spec-kit/
  mattpocock lo *"làm gì"*. Cách kết hợp hợp lý: **harness (điều phối) → gstack/spec-kit/mattpocock (thực thi)**.
- **Khi nào cân nhắc**: việc lớn, nhiều agent song song, chạy dài, cần cấu trúc team rõ. Project khởi đầu
  bình thường **không cần** — để gọn (*Simplicity First*).
- **⚠ Nếu dùng**: (1) **chồng khái niệm "team" với gstack** (gstack đã mô phỏng eng team) → phân vai rõ,
  đừng để 2 mô hình team đá nhau; (2) harness **sinh file** vào `.claude/` → coi chừng đè skill vendored /
  toolkit đã cài.
- **Trạng thái**: chỉ ghi nhận làm đường nâng cấp. Chưa fork/pin, chưa route mặc định. Khi nào thực sự cần
  orchestration đa agent thì mới cân nhắc đưa lên thành external toolkit đầy đủ.
