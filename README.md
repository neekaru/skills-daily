# skills-daily

Agent skills repository untuk enhanced problem-solving dan komunikasi yang efisien.

## Installation

```bash
npx skills add neekaru/skills-daily
```

## Skills

### hyper-focus-solver
Problem-solving skill yang cepat, high-signal, tanpa fluff. Dirancang untuk menghindari AI responses yang ngawur atau menambahkan fungsi yang tidak diperlukan. 

**Fitur:**
- Zero fluff, langsung ke solusi (ADHD-optimized)
- Anti-ngawur: identifikasi root cause sebelum fix
- Mahir dalam agentic tooling & MCP
- Output terstruktur: [Diagnosis] → [Action] → [Verification]

Bisa standalone atau paired dengan `deep-thinking`.

### deep-thinking
Skill untuk debugging complex/unintuitive bugs dan interpretasi ambiguous requests. Memaksa fase research yang rigorous untuk mencegah kesimpulan yang obvious tapi salah.

**Fitur:**
- Research phase: trace flow, form multiple hypotheses (min 3)
- Verification phase: aktif membuktikan asumsi salah sebelum accept
- Execution phase: precise, surgical action
- Output format: [Deep Research] → [Root Cause] → [Precise Action]

Bekerja exceptionally well dengan `hyper-focus-solver`.

### smart-analyzer
Deep-analysis skill untuk diagnosing bugs, tracing data flows, dan investigative tasks dengan minimal token usage. Production-safe design.

**Fitur:**
- Deep analysis: trace data flow, multiple hypotheses, falsify assumptions
- Light investigative tasks: database queries, log inspection, API debugging
- Token-efficient: surgical queries, targeted reads
- Production-safe: read-only by default, permission-required untuk mutations

**Scope:**
- Gunakan saat perlu *execute* untuk gather evidence (query, grep, read logs)
- Berbeda dengan deep-thinking yang pure reasoning tanpa execution

Bisa hand off ke skill lain: deep-thinking (tradeoffs), skill-creator, docx, xlsx sesuai kebutuhan.

## Use Cases

- **hyper-focus-solver**: Light/medium/heavy problem-solving, fix erratic AI thinking, surgical code fixes
- **deep-thinking**: Complex bugs dengan root cause tidak jelas, ambiguous instructions, architectural reasoning
- **smart-analyzer**: Deep debugging dengan live evidence gathering, database/log/API investigation, production diagnostics

Semua skills bisa digunakan **bersamaan** atau **standalone** sesuai kebutuhan task.
