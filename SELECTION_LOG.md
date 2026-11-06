# SELECTION_LOG.md (Task 7)

Fill in the "Selected" column by running the command with `--collect-only -q` and counting
test lines, or by reading the summary line. Then write one sentence on when you would use it.

| Command | Selected | When would a team run this? |
|---|---|---|
| `python -m pytest -m smoke` | TODO | TODO |
| `python -m pytest -m "regression and not slow"` | TODO | TODO |
| `python -m pytest -m boundary` | TODO | TODO |
| `python -m pytest -k username` | TODO | TODO |
| `python -m pytest -k "grade and not raises"` | TODO | TODO |
| `python -m pytest tests/test_cart.py::TestCartTotal::test_empty_cart_total_is_zero` | TODO | TODO |

## Questions
1. Temporarily change one `@pytest.mark.smoke` to `@pytest.mark.smok` (typo) and run pytest. What happens and what exit code do you get? Why is that useful? (Undo the typo afterwards!)
   TODO
2. What exit code did a run that selected zero tests give? (Try `-k zzzz`.) TODO
3. Which test is your xfail, and what would you see if the bug were fixed (strict=True)? TODO

## Reading a failure
I changed the expected word count in `test_count_words_simple_sentence` from 4 to 5, and pytest marked the line with `>`, then printed `assert 4 == 5` followed by `where 4 = count_words("quality is everyone's job")`. That introspection shows the actual value (4) came from the function call and the expected value (5) was the one I wrote, so I could tell straight away that the test was wrong, not the code, without adding any print statements.
