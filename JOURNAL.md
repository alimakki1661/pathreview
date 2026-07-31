## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/158

**Issue title:** review_service unit tests misconfigure async mocks — 13 of 19 tests fail

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**

The unit tests for `core/services/review_service.py` incorrectly use asynchronous mocks for SQLAlchemy result objects. Although the database session's `execute()` method is asynchronous, result methods such as `scalars()`, `first()`, and `all()` are synchronous. This mismatch causes coroutine objects to be returned where the service expects reviews or lists, resulting in 13 of the 19 tests failing. A successful fix will configure the mocks to match SQLAlchemy's actual behavior and make all review service unit tests pass without unnecessarily changing the production service.

**Branch name:** `test/158-review-service-async-mocks`

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger

### Issue selection notes

This issue has a clearly defined problem, a limited scope, and an existing test file that reproduces the failures. The expected changes should primarily affect `tests/unit/test_review_service.py` rather than multiple parts of the application. The fix can be verified by running the 19 review service unit tests and confirming that they all pass. Because the affected code and success criteria are clear, this Tier 1 issue is realistic to complete during the Module 3 timeline.

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/alimakki1661/pathreview/commit/44768c9

**Reproduction summary:**

I reproduced issue #158 by running `tests/unit/test_review_service.py`. The run produced 13 failed tests and 6 passed tests because synchronous SQLAlchemy result methods were incorrectly represented by asynchronous mocks.

**PLAN.md link:** https://github.com/alimakki1661/pathreview/blob/test/158-review-service-async-mocks/PLAN.md

**Walkthrough video (recommended):** Not recorded.

**Blockers or open questions:**

No current blockers. During implementation, I will verify the correct result access pattern for methods that execute multiple database queries.

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**

Reproduced the 13 failing review-service tests, identified the incorrect async result mocks, and updated the tests so `db.execute()` remains asynchronous while SQLAlchemy result methods are synchronous.

**Next steps:**

Run formatting, linting, targeted tests, and the broader project checks; then document any pre-existing failures and finalize the pull request.

**Blockers:**

The repository's type-check step reports existing missing-annotation errors in `core/services/review_service.py` and `tests/unit/test_review_service.py`.

---

### Check-in 2 (end of week)

**PR link:** https://github.com/ascherj/pathreview/pull/415

**Branch:** `test/158-review-service-async-mocks`

**What you built:**

Updated the review-service unit-test mocks to match SQLAlchemy's real async boundary: `db.execute()` is awaited, while the returned result object and its `scalars()`, `first()`, and `all()` methods behave synchronously. No production service code was changed.

**Tests added or updated:**

Updated `tests/unit/test_review_service.py` to cover single-result, empty-result, list, pagination, ownership, and multiple-query behavior. The targeted suite passes all 19 tests.

**Self-review confirmation:** [ ] `make check` passes  [ ] `make test-unit` passes

Targeted verification completed: Ruff passed, Black passed, and `pytest tests/unit/test_review_service.py -q` passed with 19 tests. The full `make check` and `make test-unit` commands still need final verification; the observed mypy errors are documented as pre-existing and unrelated to this test-mock change.

**Draft PR feedback received from:** none
