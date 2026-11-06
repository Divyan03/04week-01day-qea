# SELECTION_LOG.md (Task 7)

Fill in the "Selected" column by running the command with `--collect-only -q` and counting
test lines, or by reading the summary line. Then write one sentence on when you would use it.

| Command | Selected | When would a team run this? |
|---|---|---|
| `python -m pytest -m smoke` | 7 | On every commit or pull request as a fast gate that tells us within seconds whether the basics are broken. |
| `python -m pytest -m "regression and not slow"` | 12 | Before merging or on each push to CI, to get broad regression coverage without waiting for the slow tests. |
| `python -m pytest -m boundary` | 11 | When changing validation or limit logic, to check the edge values such as 0, 100, empty input and exact lengths. |
| `python -m pytest -k username` | 7 | While working on the username feature, to run only the tests whose names mention it. |
| `python -m pytest -k "grade and not raises"` | 3 | To check the grade-to-letter mapping on its own, skipping the error-handling tests. |
| `python -m pytest tests/test_cart.py::TestCartTotal::test_empty_cart_total_is_zero` | 1 | To rerun a single test by its exact ID while debugging a failure. |

## Questions
1. Temporarily change one `@pytest.mark.smoke` to `@pytest.mark.smok` (typo) and run pytest. What happens and what exit code do you get? Why is that useful? (Undo the typo afterwards!)
   Pytest stopped with `'smok' not found in `markers` configuration option` as a collection error, ran no tests, and exited with code 2. This is useful because `--strict-markers` turns a misspelled marker into a loud failure, so a test can't silently drop out of the `smoke` group.
2. What exit code did a run that selected zero tests give? (Try `-k zzzz`.) Exit code 5, meaning no tests were collected (all 32 were deselected). CI treats it as a failure, so a selection that matches nothing doesn't look like a pass.
3. Which test is your xfail, and what would you see if the bug were fixed (strict=True)? `test_truncate_text_exactly_max_len_is_unchanged` in `tests/test_text_utils.py`. It documents a known bug where `truncate` cuts text that fits exactly (it uses `>=` instead of `>`). Once the bug is fixed the test would unexpectedly pass, and because of `strict=True` pytest would report it as `XPASS(strict)` and fail the run, telling us to remove the xfail marker.

## Reading a failure
I changed the expected word count in `test_count_words_simple_sentence` from 4 to 5, and pytest marked the line with `>`, then printed `assert 4 == 5` followed by `where 4 = count_words("quality is everyone's job")`. That introspection shows the actual value (4) came from the function call and the expected value (5) was the one I wrote, so I could tell straight away that the test was wrong, not the code, without adding any print statements.
