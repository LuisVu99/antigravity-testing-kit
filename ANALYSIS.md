ANALYSIS - Antigravity Testing Kit

Phần 1 - Tóm tắt dự án (≤ 1 trang)

- Repo này dùng để làm gì?
  - Đây là một "Agent Kit" dành cho Tester (Manual + Automation) giúp tích hợp AI Agent vào quy trình kiểm thử: chứa Rules (quy tắc), Skills (kỹ năng), Workflows (kịch bản), prompt mẫu, và scripts hỗ trợ (ví dụ chuyển Markdown → Excel, tích hợp Jira/Google Sheets). Mục đích: sẵn sàng cài vào dự án để AI trên nền Antigravity tuân thủ chuẩn QA/Automation của nhóm.
- Ai sẽ sử dụng nó?
  - Manual Testers, Automation Engineers, QA Leads, và bất kỳ người muốn dùng AI Agent để sinh testcases, generate script (Playwright/Selenium), review code/test reports, hoặc tích hợp với Jira/Sheets.
- Khi nào nên sử dụng nó?
  - Khi muốn: tự động hóa việc sinh test cases; chuyển manual → automation; dựng framework Playwright/Selenium; tích hợp AI theo quy tắc dự án; chuẩn hóa hành vi AI Agent trong Antigravity.
- Giá trị lớn nhất của repo này là gì?
  - Tạo bộ quy tắc + workflow chuẩn (POM, locator strategy, smart waits) để AI sinh code/testcases đáng tin cậy và phù hợp với thực tế QA, giảm sai sót, tăng tốc delivery.

Phần 2 - Giải thích cấu trúc thư mục

Cấu trúc chính (rút gọn):

antigravity-testing-kit/
├── .agent/
│   ├── rules/
│   ├── skills/
│   └── workflows/
├── plans/
│   ├── automation/
│   ├── manual/
│   └── cross-module/
├── practices/
│   ├── requirements/
│   └── testcases/
├── prompt_templates/
├── scripts/
│   ├── convert_excel/
│   └── integrations/
│       ├── google_sheet/
│       └── jira/
├── GEMINI.md
├── RULE_GLOBAL.md
└── TIPS_QUOTA.md

Mỗi folder/file: chức năng — khi dùng — rủi ro khi xóa

- `.agent/rules/`
  - Chức năng: chứa quy tắc bắt buộc cho AI (ví dụ POM, cách chọn locator, quy tắc chờ).
  - Khi dùng: mỗi lần AI Agent sinh script hoặc thao tác UI, hệ thống sẽ validate theo rule này.
  - Nếu xóa: AI có thể sinh script sai chuẩn, locator không an toàn, nhiều flakiness; mất tính nhất quán.
- `.agent/skills/`
  - Chức năng: tập các "kỹ năng" (modes) AI có thể gọi — ví dụ: `locator-healer`, `test-data-generator`, `framework-architect`.
  - Khi dùng: Workflows gọi các skill để thực hiện tác vụ chuyên biệt.
  - Nếu xóa: Workflows mất khả năng thực thi các bước chuyên môn, giảm tự động hóa.
- `.agent/workflows/`
  - Chức năng: các slash command/kịch bản step-by-step (ví dụ /generate_automation_from_testcases).
  - Khi dùng: kích hoạt khi user gửi prompt tương ứng trong Antigravity.
  - Nếu xóa: mất các flow tự động; user phải viết prompt thủ công từng bước.
- `plans/automation/`
  - Chức năng: quy trình 6 bước để sinh automation scripts (chi tiết hướng dẫn từng bước).
  - Khi dùng: cho tác vụ phức tạp cần tuần tự (context → script → review).
  - Nếu xóa: mất hướng dẫn có cấu trúc; người dùng mới khó thực hiện end-to-end.
- `plans/manual/`
  - Chức năng: quy trình 6 bước sinh manual test cases theo AI-RBT (Risk-Based Testing).
  - Khi dùng: sinh testcases manual chất lượng.
  - Nếu xóa: mất quy trình cho manual testers.
- `prompt_templates/`
  - Chức năng: kho prompt mẫu (copy → paste → send). Có các mẫu tạo requirements, testcases, script Playwright/Selenium, generate test data…
  - Khi dùng: nhanh chóng gửi prompt đúng format.
  - Nếu xóa: mất templates tiện lợi, tăng công việc soạn prompt.
- `scripts/convert_excel/`
  - Chức năng: công cụ chuyển Markdown testcases → Excel (layout phù hợp).
  - Khi dùng: export testcases thành Excel để chia sẻ/báo cáo.
  - Nếu xóa: mất chuyển đổi tiện dụng.
- `scripts/integrations/jira/` và `scripts/integrations/google_sheet/`
  - Chức năng: kết nối Jira, Google Sheets (đọc/ghi, lấy requirements, cập nhật test results).
  - Khi dùng: tích hợp CI/PLM và báo cáo.
  - Nếu xóa: mất tích hợp, phải làm thủ công.
- `GEMINI.md`, `RULE_GLOBAL.md`, `TIPS_QUOTA.md`
  - Chức năng: tài liệu quy ước toàn cục, tips tối ưu token/quota, rule chung.
  - Khi dùng: tham khảo chính sách, quy tắc khi cấu hình Agent.
  - Nếu xóa: mất nguồn quy tắc tổng quát, nguy cơ cấu hình không nhất quán.
- `README.md` (gốc)
  - Chức năng: mô tả tổng quan, hướng dẫn cài đặt nhanh, cấu trúc.
  - Khi dùng: entrypoint cho người mới.
  - Nếu xóa: người mới sẽ lạc hướng.

Phần 3 - Bản đồ mối quan hệ giữa các thành phần

- `.agent/workflows/*` → gọi `.agent/skills/*` → áp dụng `.agent/rules/*` (trước khi trả kết quả)
- `plans/automation/QUICK_START.md` → hướng dẫn gọi `/generate_automation_from_testcases` workflow
- `prompt_templates/prompt_04_generate_script_playwright.txt` → được workflow script-generator dùng làm mẫu input
- `scripts/integrations/jira/*` → cung cấp requirements/testcases nguồn cho `plans/` hoặc workflows
- `scripts/convert_excel/md_to_xlsx.js` → dùng `practices/testcases/` (markdown) làm input → xuất Excel
- `RULE_GLOBAL.md` / `GEMINI.md` → là nguồn quy tắc tham chiếu cho `.agent/rules/*` và workflows

Bảng ví dụ:
- `.agent/workflows/generate_automation` → `.agent/skills/automation_engine`, `.agent/rules/pom_rules`
- `plans/manual/QUICK_START.md` → `prompt_templates/prompt_02_generate_test_cases.txt`
- `scripts/integrations/jira/*.js` → Jira API, `practices/requirements/`
- `prompt_templates/prompt_05_convert_manual_to_automation.txt` → `.agent/workflows/convert_manual_to_automation`

Phần 4 - Luồng hoạt động từ đầu đến cuối

Ví dụ: user muốn "Tạo Playwright script từ test case":
1. User (Tester) nhập yêu cầu vào Antigravity (ví dụ: "Generate Playwright script for TC #123").
2. Antigravity map input → kích hoạt Workflow tương ứng (ví dụ `/generate_automation_from_testcases`).
3. Workflow kiểm tra áp dụng Rule global (`RULE_GLOBAL.md`, `.agent/rules/*`) — ví dụ bắt buộc: "dùng POM", "không dùng absolute XPath".
4. Workflow gọi Skill chuyên trách (ví dụ `framework-architect` để chọn template, `locator-healer` để tìm locator, `test-data-generator` để tạo dữ liệu).
5. AI Agent xử lý: đọc test case (từ `practices/testcases/` hoặc từ prompt), áp dụng rules khi sinh code, gọi skill để thực thi từng phần, tạo file script (Playwright TS) + POM class.
6. Kết quả trả về: code, báo cáo kiểm tra rule violations (nếu có), gợi ý fix, và file có thể tải xuống hoặc commit.

Phần 5 - Sơ đồ khối (Mermaid)

```mermaid
flowchart TD
  User[User nhập yêu cầu] -->|slash command / prompt| Workflow[Workflow (/.agent/workflows)]
  Workflow --> ApplyRules[Áp dụng Rules (.agent/rules)]
  Workflow --> CallSkills[ gọi Skills (.agent/skills) ]
  CallSkills --> AI[AI Agent xử lý]
  ApplyRules --> AI
  AI --> Output[Kết quả: code / testcases / report]
  Output -->|save| Practices[practices/testcases]
  Output -->|export| Scripts[ scripts/convert_excel hoặc integrations ]
```

Phần 6 - Giải thích theo ví dụ thực tế

1) "Tạo test case (Manual) từ requirement"
- File tham gia: `prompt_templates/prompt_01_generate_requirements.txt`, `plans/manual/QUICK_START.md`, `practices/requirements/`
- Workflow: `/generate_manual_testcases_rbt` (plans/manual hướng dẫn 6 bước)
- Skill: `requirements-analyzer`, `rbt-generator`
- Rule kiểm tra: `RULE_GLOBAL.md` (mẫu format TC, độ rõ ràng)
- Kết quả: file TC markdown lưu vào `practices/testcases/`

2) "Tạo Playwright script từ một Test Case"
- File tham gia: `prompt_templates/prompt_04_generate_script_playwright.txt`, `plans/automation/QUICK_START.md`
- Workflow: `/generate_automation_from_testcases`
- Skill: `framework-architect`, `locator-healer`, `test-data-generator`
- Rule kiểm tra: `.agent/rules/pom_rules`, `.agent/rules/locator_strategy`
- Kết quả: `tests/` + `page_objects/` (POM) code, báo cáo violations

3) "Review bug report / automation code review"
- File tham gia: `prompt_templates/prompt_06_review_automation_code.txt`, `practices/testcases/`
- Workflow: `/review_automation_code`
- Skill: `code_linter`, `flaky-analyzer`
- Rule kiểm tra: coding standard rule trong `.agent/rules` và `GEMINI.md`
- Kết quả: danh sách issues, gợi ý fix, đề xuất test data thêm

Phần 7 - Những file quan trọng nhất (Top 10)

1. `.agent/rules/*` (thư mục)
   - Tại sao: đảm bảo tính an toàn và chuẩn mực khi AI sinh code/test.
   - Nếu thiếu: mất trấn áp chất lượng, code không nhất quán.
2. `.agent/workflows/*` (thư mục)
   - Tại sao: chứa các kịch bản thực thi; là điểm entry cho user.
   - Nếu thiếu: không có flow tự động – mất giá trị chính của kit.
3. `.agent/skills/*` (thư mục)
   - Tại sao: implement chuyên môn (locator healing, test data...).
   - Nếu thiếu: workflows không thể thực hiện chức năng chuyên sâu.
4. `plans/automation/QUICK_START.md`
   - Tại sao: hướng dẫn 6 bước để tạo automation theo chuẩn.
   - Nếu thiếu: người mới khó làm end-to-end.
5. `plans/manual/QUICK_START.md`
   - Tại sao: hướng dẫn AI-RBT cho manual testers.
   - Nếu thiếu: manual testers mất quy trình rõ ràng.
6. `prompt_templates/prompt_04_generate_script_playwright.txt`
   - Tại sao: template input chuẩn để tạo Playwright script.
   - Nếu thiếu: tăng lỗi do prompt thiếu thông tin.
7. `scripts/convert_excel/md_to_xlsx.js`
   - Tại sao: chuyển testcases → Excel, rất thực tế cho QA teams.
   - Nếu thiếu: mất export thuận tiện.
8. `scripts/integrations/jira/` (thư mục)
   - Tại sao: tích hợp với Jira/Xray — quan trọng cho tổ chức lớn.
   - Nếu thiếu: mất automation với PLM.
9. `RULE_GLOBAL.md`
   - Tại sao: quy tắc toàn cục, tham chiếu chung.
   - Nếu thiếu: thiếu chỉ dẫn tổng quát cho agent.
10. `README.md` (gốc)
   - Tại sao: entrypoint; giải thích tổng quan + cách dùng nhanh.
   - Nếu thiếu: người mới không biết bắt đầu từ đâu.

Phần 8 - Hướng dẫn cho người mới (30 phút để hiểu repo)

Thứ tự đọc (ưu tiên — 30 phút):

1. Mở README.md — đọc nhanh 2-3 phút (tổng quan).
2. Mở plans/manual/QUICK_START.md — 5 phút (xem quy trình AI-RBT).
3. Mở plans/automation/QUICK_START.md — 5 phút (quy trình 6 bước tạo script).
4. Mở `.agent/` folder: đọc `rules/` và `workflows/` (khoảng 8 phút) — hiểu cách agent sẽ tuân theo quy tắc.
5. Mở `prompt_templates/prompt_04_generate_script_playwright.txt` và `prompt_templates/prompt_02_generate_test_cases.txt` — 5 phút (xem ví dụ prompt).
6. Tùy chọn: xem `scripts/convert_excel/README.md` hoặc `scripts/integrations/jira/README.md` nếu cần tích hợp.

Những file có thể bỏ qua lúc đầu: các README con chi tiết, toàn bộ `practices/` (trừ khi muốn xem ví dụ testcases), và file cấu hình nâng cao trong integrations.

Phần 9 - Tóm tắt một trang: "Nếu chỉ nhớ 5 điều"

- Đây là một kit chuẩn hoá AI Agent cho QA: chứa Rules, Skills, Workflows để AI sinh testcases và automation theo chuẩn.
- `.agent` là lõi: `rules/` kiểm soát chất lượng; `workflows/` là entrypoint; `skills/` là công cụ chuyên môn.
- `plans/` cung cấp quy trình 6 bước (manual & automation) — dùng để thực hiện tác vụ end-to-end.
- `prompt_templates/` giúp bạn soạn prompt đúng và nhanh — copy → paste để bắt đầu.
- `scripts/` (Jira, Google Sheets, convert Excel) giúp tích hợp và xuất/nhập dữ liệu thực tế trong quy trình QA.


Nếu bạn muốn, tôi có thể tiếp tục: tạo link trực tiếp tới các file quan trọng trong `ANALYSIS.md` hoặc thêm checklist thao tác nhanh để thử một workflow trong Antigravity.