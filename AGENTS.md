# AI Developer -- Regulatory Code Analysis Agents

## Project Overview

This project provides a suite of specialized OpenCode agents for **AI-powered regulatory code analysis** in the financial sector. The agents work as a pipeline to:

1. **Trace attribute-level data lineage** through code (SQL, Python, PySpark, Java, Scala)
2. **Verify code implementation** against regulatory documents (ECB, EBA, IFRS, Basel)
3. **Map critical data attributes (CDAs)** for regulatory calculations

## Agent Pipeline

```
[Regulatory Document]     [Source Code]
        │                       │
        └───────┬───────────────┘
                ▼
    ┌─────────────────────┐
    │  @lineage-analyst   │  → Attribute-level data lineage report
    └─────────┬───────────┘
              ▼
    ┌─────────────────────┐
    │ @regulatory-verifier│  → Compliance verification matrix
    └─────────┬───────────┘
              ▼
    ┌─────────────────────┐
    │    @cda-mapper       │  → Critical data attribute mapping
    └─────────────────────┘
```

## Build & Test Commands

```bash
# Install OpenCode
npm install -g opencode

# Launch with local models (air-gapped)
opencode

# Invoke specific agents
# @lineage-analyst  -- trace data lineage in code
# @regulatory-verifier -- verify code against regulation
# @cda-mapper -- map critical data attributes
```

## Code Style Guidelines

- Output reports in Markdown using the templates in `templates/`
- Use structured tables for lineage and mapping outputs
- Always include source file paths and line numbers in references
- Use `[PASS]`, `[FAIL]`, `[PARTIAL]`, `[NOT FOUND]` status markers

## Supported Languages

| Language | File Types | Analysis Depth |
|----------|-----------|----------------|
| SQL | `.sql`, `.hql`, `.ddl` | Full: CTEs, subqueries, joins, window functions |
| Python | `.py` | Full: pandas, PySpark, numpy transformations |
| PySpark | `.py` | Full: DataFrame API, SQL expressions, UDFs |
| Java | `.java` | Moderate: method chains, Stream API |
| Scala | `.scala` | Moderate: Spark Scala API |
| YAML/JSON | `.yaml`, `.json` | Config: source definitions, mappings |

## Boundaries & Restrictions

### Always Do
- Include file paths and line numbers in every lineage reference
- Trace data from source system through every transformation to output
- Flag any attribute that cannot be traced to a source (orphan attributes)
- Cross-reference every regulatory requirement with its code implementation

### Ask First
- Before modifying any source code files
- Before making assumptions about business logic not documented in regulations
- When an attribute has ambiguous lineage (multiple possible sources)

### Never Do
- Never modify production code or regulatory documents
- Never skip transformation steps in lineage (every hop must be documented)
- Never mark a requirement as PASS without identifying the specific code implementing it
- Never ignore hardcoded values or magic numbers in calculations

## Project Structure

```
ai-developer-agents/
├── AGENTS.md                    # This file -- project context for agents
├── opencode.json                # OpenCode config with MCP tools
├── .opencode/
│   ├── agents/                  # Agent definitions
│   │   ├── lineage-analyst.md
│   │   ├── regulatory-verifier.md
│   │   └── cda-mapper.md
│   └── skills/                  # Reusable skill templates
│       ├── trace-lineage/
│       ├── verify-regulation/
│       └── map-cda/
├── templates/                   # Output report templates
├── examples/                    # Worked example (IFRS 9 LGD)
│   ├── regulation/
│   ├── code/
│   └── output/
└── README.md
```

## Regulatory Framework Context

These agents are designed for the **European financial sector** regulatory landscape:

| Regulation | Issuer | Key Calculations |
|-----------|--------|-----------------|
| IFRS 9 | IASB | ECL, PD, LGD, EAD, staging |
| CRR/CRD (Basel III/IV) | ECB/EBA | RWA, capital ratios, leverage |
| BCBS 239 | BCBS | Data lineage, data quality, aggregation |
| FRTB | BCBS | Market risk capital, sensitivities |
| AnaCredit | ECB | Credit data reporting attributes |
| FINREP/COREP | EBA | Financial/supervisory reporting |

## Security Considerations

- All analysis runs locally -- no code or regulatory documents leave the network
- Agent outputs may contain sensitive regulatory interpretations -- treat as confidential
- Use local models (Ollama) for air-gapped environments
- Never include actual production data in lineage examples
