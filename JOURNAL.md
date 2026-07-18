\## Week 7 — Issue selection



\*\*Issue link:\*\* https://github.com/ascherj/pathreview/issues/158



\*\*Issue title:\*\* review\_service unit tests misconfigure async mocks — 13 of 19 tests fail



\*\*Tier:\*\* \[x] Tier 1  \[ ] Tier 2  \[ ] Tier 3



\*\*Problem summary:\*\*

The unit tests for `core/services/review\_service.py` incorrectly use asynchronous mocks for SQLAlchemy result objects. Although the database session's `execute()` method is asynchronous, result methods such as `scalars()`, `first()`, and `all()` are synchronous. This mismatch causes coroutine objects to be returned where the service expects reviews or lists, resulting in 13 of the 19 tests failing. A successful fix will configure the mocks to match SQLAlchemy's actual behavior and make all review service unit tests pass without unnecessarily changing the production service.



\*\*Branch name:\*\* test/158-review-service-async-mocks



\*\*Setup confirmation:\*\* \[x] App runs locally at localhost:5173



\*\*Cohort ledger:\*\* \[x] Issue added to cohort ledger



\### Issue selection notes



This issue has a clearly defined problem, a limited scope, and an existing test file that reproduces the failures. The expected changes should primarily affect `tests/unit/test\_review\_service.py` rather than multiple parts of the application. The fix can be verified by running the 19 review service unit tests and confirming that they all pass. Because the affected code and success criteria are clear, this Tier 1 issue is realistic to complete during the Module 3 timeline.

