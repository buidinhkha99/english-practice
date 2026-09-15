# Phase 0 — Foundation

## Context Links
- PRD §9 Kiến trúc kỹ thuật, §8 Mô hình dữ liệu
- [plan.md](plan.md)

## Overview
- **Priority:** P0 — chặn mọi phase khác
- **Status:** ⬜ Chưa bắt đầu
- Dựng khung dự án chạy được đầu-cuối: scaffold Next.js, kết nối Postgres, migration đầu tiên,
  đăng nhập được, deploy lên Vercel. Không có tính năng nghiệp vụ nào ở phase này.

## Key Insights
- Ràng buộc kiến trúc quan trọng nhất phải được thiết lập **ngay từ đây**: mọi logic nghiệp vụ
  nằm ở `lib/`, không ở React component. Phase 7 (React Native) phụ thuộc hoàn toàn vào điều này.
- Schema có `user_id` từ migration đầu — thêm sau sẽ phải migrate đau.
- Bảng catalog (`lexemes`, `phrases`, `grammar_patterns`, `senses`) **không** gắn `user_id`,
  để cache enrich dùng chung khi mở multi-user.

## Requirements
**Functional**
- Đăng nhập bằng Google, tạo bản ghi `users` với `level_cefr` mặc định và `settings` jsonb
- Migration chạy được cả local lẫn production
- Trang Home rỗng có layout + navigation

**Non-functional**
- TypeScript strict mode
- Lint + typecheck chạy được bằng một lệnh
- Deploy preview tự động trên mỗi push

## Architecture
```
app/           route handlers, pages, layouts  ← KHÔNG chứa logic nghiệp vụ
lib/
  db/          schema Drizzle, migrations, client
  auth/        cấu hình Auth.js
  nlp/         (Phase 1)
  llm/         (Phase 1)
  srs/         (Phase 2)
components/    UI thuần, không gọi DB trực tiếp
```

## Related Code Files
**Tạo mới**
- `package.json`, `tsconfig.json`, `next.config.ts`, `drizzle.config.ts`
- `lib/db/schema.ts` — bảng `users`, `sources`, `source_sentences`
- `lib/db/index.ts` — Drizzle client
- `lib/auth/config.ts` — Auth.js v5
- `app/layout.tsx`, `app/page.tsx`, `app/api/auth/[...nextauth]/route.ts`
- `.env.example`, `.github/workflows/ci.yml`
- `ck-docs/code-standards.md`, `ck-docs/system-architecture.md`

## Implementation Steps
1. `npx create-next-app@latest` — TypeScript, Tailwind, App Router, src-less layout
2. Cài `drizzle-orm` + `drizzle-kit` + `@neondatabase/serverless`; tạo project Neon, lấy connection string
3. Viết `lib/db/schema.ts` cho `users` / `sources` / `source_sentences` (§8 PRD) — **có `user_id`**
4. Sinh + chạy migration đầu tiên, xác nhận trên Neon
5. Cài `next-auth@beta`, cấu hình Google provider, adapter Drizzle
6. Cài shadcn/ui, dựng layout + nav + trang Home rỗng
7. Thêm CI: `tsc --noEmit` + `eslint` + `drizzle-kit check`
8. `vercel link` + `vercel env add` cho `DATABASE_URL`, `AUTH_SECRET`, `AUTH_GOOGLE_*`
9. Deploy production, xác nhận đăng nhập thật chạy được
10. Viết `ck-docs/code-standards.md` chốt ràng buộc kiến trúc (logic ở `lib/`, file < 200 dòng)

## Todo List
- [ ] Scaffold Next.js + TypeScript strict + Tailwind
- [ ] Neon project + `DATABASE_URL`
- [ ] Drizzle schema `users` / `sources` / `source_sentences`
- [ ] Migration đầu tiên chạy được local + prod
- [ ] Auth.js v5 + Google provider
- [ ] shadcn/ui + layout + nav
- [ ] CI: typecheck + lint
- [ ] Vercel link + env vars + deploy
- [ ] `ck-docs/code-standards.md` + `system-architecture.md`

## Success Criteria
- Đăng nhập Google trên URL production thật, thấy email mình trong bảng `users`
- `npm run build` sạch, CI xanh
- Không có secret nào trong git (`git log -p | grep -i` cho key/token ra rỗng)

## Risk Assessment
| Rủi ro | Giảm thiểu |
|---|---|
| Auth.js v5 còn beta, API đổi | Pin exact version, không dùng API experimental |
| Neon free tier ngủ đông → cold start chậm | Chấp nhận ở dev; đo lại ở Phase 2 khi Home cần < 1.5s |
| Schema thiếu `user_id` ở bảng nào đó | Checklist review trước khi chạy migration đầu |

## Security Considerations
- `.env` vào `.gitignore` ngay (đã có sẵn từ commit đầu)
- Secrets chỉ qua `vercel env`, không hard-code
- `AUTH_SECRET` sinh ngẫu nhiên, khác nhau giữa preview và production

## Next Steps
Phase 1 — Capture & Analyze. Cần schema `sources` + `source_sentences` đã sẵn sàng từ phase này.
