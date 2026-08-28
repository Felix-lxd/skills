# Validation Phase Details

Phase numbering follows SKILL.md gate chain: P0 plan detection, P1 requirement conformance (defined in SKILL.md), P2~P6 and P8 below, P7 execution-effect simulation (defined in SKILL.md).

## Phase 2: Initiate Review (/requesting-code-review + /code-review-excellence)

At the start of each validation round, coordinate two Skills (**review mode only, no repair actions**):

**`/requesting-code-review`** handles:
- Determine review scope (changed files, commit range)
- Define review standards (based on applicable 19 dimensions)
- Prepare context packages for three-way CodeReview sub-agents

**`/code-review-excellence`** handles:
- Provide code review best practice baseline (standards, feedback format, severity grading)
- Ensure review process is constructive, avoid invalid feedback
- Define CodeReview sub-agent output quality requirements

## Phase 3: Three-Way Parallel CodeReview

Each validation round dispatches three CodeReview sub-agents simultaneously, each reviewing independently (**review mode, repair mode forbidden**):

### 1. completeness
- Does repair scope cover all issue points?
- Are other files with same name/structure missed?
- Does plan description match actual file content?
- Are line number locations precise?

### 2. correctness
- Is repair direction consistent with code authority source?
- Is replacement target value correct?
- Do repair actions conflict with each other?
- No collateral damage (e.g., ref_model not affected by model replacement)

### 3. impact
- Do other files/interfaces need synchronized updates?
- Are cross-chapter references consistent within documents?
- Are cross-module (three-side) documents consistent?
- Are new inconsistencies introduced?

### Merge Report
After three sub-agents return:
- Group by severity: Critical Issues / Warnings / Suggestions
- Cross-perspective dedup: same issue keeps only the one with richest evidence
- Clear conclusion: Pass / Conditional Pass / Fail

## Phase 4: Receive Review Feedback (/receiving-code-review)

After three-way review merge report:
- Judge each Critical Issue: confirm for plan / mark as false positive with reasoning
- Evaluate Warnings for inclusion in current plan scope
- List Suggestions and add to plan, generate selectable list for user decision (include, defer, or abandon)
- User-selected Suggestions become repair tasks, merged with Critical/Warning items into structured repair instruction list as Phase 5 input

## Phase 5: Solution Reasonability Argumentation

After Phase 4 feedback processing, before Phase 6 multi-dimensional verification:

### 5A. Root Cause Verification
- **Diagnostic accuracy**: Does the issue description accurately reflect actual code defect?
- **Causal chain completeness**: Do repairs target root cause or symptoms?
- **Palliative vs curative**: If only treating symptoms, must mark risk and supplement root cause steps

### 5B. Alternative Comparison
For each Critical repair task:
- List at least 1 alternative (different implementation path, abstraction level, repair granularity)
- Compare dimensions: effectiveness, complexity, performance impact, maintainability, risk
- Argue why current solution is optimal (or decide to switch)

### 5C. Sufficiency & Over-Repair Detection
- **Sufficiency**: Does each measure **fully** solve the corresponding problem?
- **Over-repair**: Is repair scope minimized? Unnecessary refactoring or unrelated changes?
- **Side-effect prediction**: Will measures hold under concurrency/boundary/data growth scenarios?

### 5D. Wheel Reinvention Detection
For each plan task that creates a **new** component/function/script/tool/module:
- **Existing capability search**: Use Grep/Glob/SearchCodebase to search the codebase for equivalent or similar implementations (utility functions, shared modules, existing scripts, devtools, skills)
- **Reuse-first judgment**: If an equivalent implementation exists, the plan must be amended to reuse/extend it instead of building anew
- **New-build justification**: If new build is retained, the plan must document why existing capability is insufficient (functional gap, coupling risk, boundary mismatch)
- Output per item: ✅ no duplication / ❌ reusable existing implementation found (with file path evidence)

### 5E. Output
Generate "Solution Reasonability Report":
- Root cause verification (per item: ✅ root cause / ⚠️ palliative risk / ❌ misdiagnosis)
- Alternative comparison table (Critical items required)
- Sufficiency & over-repair detection conclusion
- Wheel reinvention detection conclusion (new-build items with reuse evidence)
- If misdiagnosis or insufficient measures found, amend plan and re-enter this Phase

## Phase 6: 19-Dimension Verification + 9 Specialist Skill Checks

### 6A. Design Consistency Layer (5 parallel)

| Skill | Dimensions | Check Points |
|-------|-----------|--------------|
| `/design-debt-review` | 7,11 | Hardcoded colors/magic numbers; existing debt exacerbated |
| `/design-md-review` | 11 | DESIGN.md contract consistency (tokens, component rules, layout) |
| `/ui-alignment-review` | 6,11 | Pixel-level UI alignment with Figma/prototype/DESIGN.md |
| `/visual-regression-review` | 7,9 | Screenshot comparison; threshold/mask config |
| `/plugin:design-review` | 11 | Custom design rule set; project-specific constraints |

### 6B. Code Quality Layer (2 parallel)

| Skill | Dimensions | Check Points |
|-------|-----------|--------------|
| `/frontend-code-review` | 4,5,7 | Frontend correctness, component design, data boundaries |
| `/architecture-review` | 10,11 | Dependency direction, module boundaries, circular deps |

### 6C. Quality Assurance & Experience Layer (2 parallel)

| Skill | Dimensions | Check Points |
|-------|-----------|--------------|
| `/accessibility-review` | 6,7 | WCAG compliance, keyboard nav, ARIA, contrast |
| `/project-experience` | 7,10 | Platform hidden limits (Feishu/Excel); scheduled task accumulation |

### 6D. 19-Dimension Item-by-Item Verification

| # | Dimension | Check Points |
|---|-----------|--------------|
| 1 | Operator persistence | Is operator info saved to DB at each step? |
| 2 | Measure completeness | Are all viable optimization measures enumerated? |
| 3 | Optimal solution confirmed | At least 1 alternative with trade-off comparison? |
| 4 | Context synchronization | Do interface calls need synchronized updates? |
| 5 | Context consistency | Is repair context consistent, call chain connected? |
| 6 | Meets expectations | Does repair result match user's original intent? |
| 7 | No new issues | No new bugs or inconsistencies? |
| 8 | No garbled text | No encoding issues or character corruption? |
| 9 | No context inconsistency | No contradictions in other references? |
| 10 | Dev/maintenance variability | Consider development vs maintenance stage impact |
| 11 | Module design unchanged | Confirm module design not changed; contract consistency verified |
| 12 | Iterative verification | Re-verify after modification; unresolved issues iterate into plan |
| 13 | Plan existence | Review content must have corresponding plan |
| 14 | No unnecessary unification | Handle processes/interfaces independently; reuse only when confirmed |
| 15 | Pass without execution | Plan pass only means plan quality meets the standard, no execution triggered |
| 16 | Root cause verification | Do repairs target root cause? Is diagnosis accurate? |
| 17 | Measure sufficiency | Does each measure fully solve its problem? |
| 18 | Over-repair detection | Is repair scope minimized? No unnecessary changes? |
| 19 | No wheel reinvention | Before adding new components/functions/scripts/tools, has existing codebase been searched for equivalent implementations? Plan must reuse existing utilities/modules/skills instead of duplicating them; any new build must justify why existing capability is insufficient |

## Phase 8: Final Review Gate (/ultra-review)

After all Phase 6 checks and Phase 7 simulation, invoke `/ultra-review` as final quality gate (**final review only, no execution**):
- Aggregate all 14 Skill component results
- Confirm each component has zero errors, zero anomalies
- Final ruling on any residual Warnings: block / exempt and release
- Output final review conclusion

## Loop Termination Conditions

Validation passes only when **all** of the following are met:

1. **Phase 1**: No omissions, no deviations, no unannotated out-of-scope changes; all Fix items traceable to original or derived requirements (ambiguous items recorded for user confirmation); derived requirement search coverage complete
2. **Phase 3~4**: Three-way CodeReview zero Critical Issues; all feedback handled or ruled as false positive
3. **Phase 5**: Root cause verified, Critical items have alternative comparison, no over-repair, no wheel reinvention (new capabilities confirmed to have no equivalent reusable implementation)
4. **Phase 6**: 19 dimensions all ✅ (⚠️ suggestions non-blocking); 9 specialist Skills no anomalies (or exempted/confirmed as expected change)
5. **Phase 7**: Simulation confirms all original problems solved, all expected functionality implemented (original + derived dual-baseline verified), no new problems introduced
6. **Phase 8**: `/ultra-review` confirms 14 Skill components zero errors, zero anomalies
7. No unresolved cross-file / cross-module consistency issues

### Post-Final-Review Mandatory Termination

**After final review passes (✅), the only legal actions are:**
1. Update plan status to `已通过`
2. Output final optimized plan
3. Output prompt: "✅ Plan validation passed. User decides whether/when to execute. This skill does not execute any repairs."
4. **Immediately terminate all flows**

**Absolutely forbidden after final review pass:**
- ❌ Auto-start executing plan repair steps
- ❌ Invoke execution Skills (`/executing-plans`, `/subagent-driven-development`)
- ❌ Modify any source code files
- ❌ Mark plan as `已执行` or `执行中`
- ❌ Suggest, imply, or proxy user execution
