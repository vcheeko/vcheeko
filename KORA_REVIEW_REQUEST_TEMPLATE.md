# KORA — Independent Technical Review Template

Use this only after the Golden Demo internal gates M1–M6 have passed.

## Review scope

**Version / commit:**  
**Review date:**  
**Reviewer:**  
**Area:** backend / systems / security / AI evaluation / product reliability

## Evidence provided

- architecture summary;
- Golden Demo acceptance matrix;
- sanitized CI/test summary;
- failure-injection results;
- restart-continuity evidence;
- known limitations;
- intentionally excluded private/security-sensitive material.

## Reviewer questions

1. Are the claims supported by the supplied evidence?
2. Can EXECUTED become VERIFIED without sufficient evidence anywhere in the reviewed flow?
3. Are authority boundaries explicit and fail-closed?
4. Can untrusted file/web/agent content escalate to authority?
5. Are retries, runtime, scope and tool use bounded?
6. Does failure preserve the last verified canonical state?
7. Can the system reconstruct the correct next action after restart?
8. Are any public claims stronger than the reviewed implementation supports?

## Findings

| ID | Severity | Finding | Evidence | Recommended action | Status |
| --- | --- | --- | --- | --- | --- |
| REV-001 |  |  |  |  | OPEN |

## Final reviewer conclusion

Choose one:

- [ ] ACCEPTABLE FOR LIMITED GOLDEN DEMO CLAIM
- [ ] ACCEPTABLE WITH DOCUMENTED LIMITATIONS
- [ ] CHANGES REQUIRED BEFORE PUBLIC CLAIM
- [ ] REVIEW INCOMPLETE

## Disclosure principle

Negative findings should not be hidden. Public evidence is stronger when it shows what was found, what was fixed and what remains open.
