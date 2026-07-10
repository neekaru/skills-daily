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

## Use Cases

- **hyper-focus-solver**: Light/medium/heavy problem-solving, fix erratic AI thinking
- **deep-thinking**: Complex bugs, ambiguous instructions, precision-critical tasks

Kedua skills bisa digunakan **bersamaan** atau **standalone** sesuai kebutuhan.
