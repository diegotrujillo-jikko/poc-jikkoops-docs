---
name: jikkoops-docs-patterns
description: Documentation patterns and workflows for the JikkoOps poc-jikkoops-docs project
version: 1.0.0
source: local-git-analysis
analyzed_commits: 50
project: poc-jikkoops-docs
---

# JikkoOps Documentation Patterns

## Commit Conventions

This project follows **conventional commits** format for git messages:

### Commit Types
- **`docs:`** - Documentation updates, translations, new guides, transcripts
- **`update`** - Direct updates to core files (CLAUDE.md, README.md)

### Examples
```
docs: translate all documentation to spanish (es-CO)
docs: add comprehensive spanish documentation and integrate 2026-04-30 transcript
docs: implement complete JikkoOps documentation structure
Update CLAUDE.md
Update README.md
```

### Best Practice
- Use `docs:` prefix for all documentation changes
- Include context in parentheses for translations: `(es-CO)`, `(en)`
- Reference transcripts by date when integrating: `2026-04-30`
- Keep commit messages concise but descriptive

## Documentation Architecture

### Folder Structure (Numbered Organization)

```
poc-jikkoops-docs/
├── 00-general/                           # Entry point, transcripts, PDFs
│   ├── 20260430-JikkoOps-Transcipt-1.md # Meeting transcript (YYYYMMDD-Topic-Counter)
│   ├── 20260429-JikkoOps-Transcript.md
│   └── Claude.pdf
│
├── 01-arquitectura/                      # System design & architecture
│   ├── 01-vision-general.md                 # English version
│   ├── vision-general-es.md              # Spanish version (-es suffix)
│   └── 02-protected-resources.md
│
├── 02-procesos/                          # Business workflows & processes
│   ├── 01-flujo-contratos.md                # Customer lifecycle
│   ├── 02-revenue-share-models.md           # English
│   └── 02-modelos-ingresos.md               # Spanish translation
│
├── 03-datos/                             # Data layer & persistence
│   ├── 02-data-model.md                     # English
│   ├── 02-modelo-datos.md                   # Spanish
│   ├── 03-feature-flags-sync.md
│   └── 04-cache-strategy.md
│
├── 04-apis/                              # Integration endpoints
│   ├── 01-endpoints-principales.md
│   └── 02-integration-events.md
│
├── 05-modulos-core/                      # Core UI modules & features
│   ├── 01-dashboard-comercial.md
│   ├── 02-gestion-contratos.md
│   ├── 03-operacion.md
│   ├── 04-facturacion.md
│   └── 05-configuracion.md
│
├── 06-riesgos-decisiones/                # Risk & architectural decisions
│   ├── 01-riesgos-identificados.md
│   └── 02-decisiones-arquitecturales.md
│
├── CLAUDE.md                             # Project instructions for Claude
├── README.md                             # User-facing introduction
├── GUIA_DOCUMENTACION_ESPANOL.md         # Spanish documentation guide
├── INDICE_DOCUMENTACION_ESPANOL.md       # Spanish documentation index
└── RESUMEN_EJECUTIVO_ES.md               # Spanish executive summary
```

### Numbering Rationale
- `00-general`: Entry point and reference materials
- `01-05`: Core system components (sequenced)
- `06`: Meta-documentation (risks, decisions)

## File Naming Conventions

### English Documentation
- **Format**: `kebab-case.md`
- **Examples**: `01-vision-general.md`, `02-protected-resources.md`, `02-revenue-share-models.md`

### Spanish Translations
- **Pattern 1**: `-es` suffix (preferred when mirroring English)
  - `01-vision-general.md` → `vision-general-es.md`
  - `02-data-model.md` → `modelo-datos-es.md` (when translated)
  
- **Pattern 2**: Direct Spanish naming (when no English version)
  - `02-modelos-ingresos.md` (standalone)
  - `02-modelo-datos.md` (standalone)

### Meeting Transcripts
- **Format**: `YYYYMMDD-Topic-Counter.md`
- **Examples**: 
  - `20260430-JikkoOps-Transcipt-1.md` (first transcript of the day)
  - `20260429-JikkoOps-Transcript.md` (no counter if only one)
  - `20260418-Claude-Best-Practices-Transcript.md`

### Root-Level Guides
- **Spanish Documentation Guide**: `GUIA_DOCUMENTACION_ESPANOL.md`
- **Spanish Index**: `INDICE_DOCUMENTACION_ESPANOL.md`
- **Spanish Summary**: `RESUMEN_EJECUTIVO_ES.md`
- **Project Instructions**: `CLAUDE.md`
- **User Introduction**: `README.md`

## Bilingual Documentation Workflow

### Adding a New Feature Document

1. **Create English version first**
   ```
   # File: 01-arquitectura/feature-name.md
   Content in English following the section structure
   ```

2. **Add to appropriate numbered folder**
   - Decide which section (01-arquitectura, 02-procesos, etc.)
   - Use kebab-case naming

3. **Create Spanish translation**
   ```
   # File: 01-arquitectura/feature-name-es.md
   (or use Spanish naming if no English mirror needed)
   Contenido en español
   ```

4. **Update indices**
   - Update `GUIA_DOCUMENTACION_ESPANOL.md` with new Spanish doc
   - Update `INDICE_DOCUMENTACION_ESPANOL.md` with both versions

5. **Update CLAUDE.md references**
   - Add section reference to CLAUDE.md if critical for Claude context

### Integration Strategy
- **Keep parallel**: English and Spanish versions in same folder
- **Cross-reference**: Both should link to each other
- **Maintain parity**: Keep translations synchronized
- **Version together**: Update both when content changes significantly

## Documentation Content Structure

### Standard Document Format

Each documentation file follows this structure:

```markdown
# [Document Title]

## Overview / Introduction
Brief explanation of the section's purpose.

## Key Concepts
- Concept 1: Definition and importance
- Concept 2: Definition and importance

## Architecture / Process Details
Detailed explanation with diagrams if needed.

## Implementation / Integration Points
How this integrates with the rest of the system.

## Examples
Real-world scenarios or code examples.

## Related Documents
Cross-references to other documentation.
```

### Examples Found
- `01-vision-general.md` - Overview of entire system
- `02-protected-resources.md` - Feature inventory and activation
- `01-flujo-contratos.md` - Complete customer lifecycle
- `02-revenue-share-models.md` - Monetization strategies

## Key Files for Claude Context

When working on JikkoOps features, Claude should read in this order:

1. **CLAUDE.md** - Project overview and instructions
2. **01-arquitectura/01-vision-general.md** - System architecture
3. **02-procesos/01-flujo-contratos.md** - Customer workflows
4. **03-datos/02-data-model.md** - Data structures
5. **Specific module docs** - As needed for the feature

## Transcript Integration Workflow

### Adding a New Transcript

1. **Save transcript from meeting**
   - Format: `YYYYMMDD-Topic-Name.md`
   - Store in `00-general/`

2. **Document meeting outcomes**
   - Key decisions made
   - Action items
   - Architecture insights

3. **Update documentation**
   - Reference transcript in related docs
   - Extract insights into main documentation
   - Note date-specific context in CLAUDE.md if needed

4. **Commit with reference**
   ```
   docs: add JikkoOps meeting transcript YYYYMMDD
   ```

## Testing & Validation

### Documentation Quality Checks
Before committing documentation changes:

- [ ] All links are valid (internal references work)
- [ ] Spanish translations match English content accuracy
- [ ] File naming follows conventions (kebab-case, -es suffix)
- [ ] No hardcoded dates except in transcripts
- [ ] CLAUDE.md is updated if adding new core concepts
- [ ] README.md is current and accurate
- [ ] Cross-references are bidirectional

### Spanish Translation Consistency
- [ ] All English docs have Spanish equivalents (or noted as English-only)
- [ ] Terminology is consistent across translations
- [ ] GUIA_DOCUMENTACION_ESPANOL.md is updated
- [ ] INDICE_DOCUMENTACION_ESPANOL.md lists all documents

## Common Workflows

### Workflow: Update Core Documentation After Meeting
```
1. Add transcript to 00-general/ (docs: add meeting transcript YYYYMMDD)
2. Extract key insights to relevant section docs
3. Update CLAUDE.md with new context if needed
4. If bilingual: Create Spanish version simultaneously
5. Update indices if new docs added
6. Single commit: docs: integrate YYYYMMDD transcript
```

### Workflow: Create New Feature Documentation
```
1. Create English version in appropriate folder (01-06)
2. Use kebab-case naming
3. Follow standard document structure
4. Create Spanish translation with -es suffix
5. Update GUIA_DOCUMENTACION_ESPANOL.md
6. Update INDICE_DOCUMENTACION_ESPANOL.md
7. If critical: Add reference to CLAUDE.md
8. Commit: docs: add [feature-name] documentation (es-CO)
```

### Workflow: Translate Existing Document to Spanish
```
1. Read English version thoroughly
2. Create new file with -es suffix or Spanish name
3. Maintain structure and section names
4. Translate terminology consistently
5. Update INDICE_DOCUMENTACION_ESPANOL.md
6. Verify links are correct
7. Commit: docs: translate [document-name] to spanish (es-CO)
```

## Notes for Claude

- This project prioritizes **clarity and comprehensiveness** for both Spanish and English audiences
- The **numbered folder structure** (00-06) is intentional for logical progression
- **Spanish translations are parity-critical** — must be kept synchronized
- **CLAUDE.md is the source of truth** for architecture context
- **Transcripts are valuable** — integrate their insights into main documentation
- **Bilingual consistency** requires updates to both language versions and both indices

## Recent Patterns Observed (April 2026)

- Heavy emphasis on Spanish documentation translation (es-CO)
- Regular integration of meeting transcripts
- Updates to CLAUDE.md with new architectural context
- Expansion of core module documentation
- Regular README.md updates for clarity

---

*Generated from analysis of 50 recent commits. Pattern confidence: HIGH*
