Implementation plan for advancement #8 (two-stage tuning flow)

Use two sequential searches in fraud_detection_precision_baseline.ipynb: a cheap broad sweep, then a focused sweep around promising regions.

1. Define budgets and acceptance rule first
- Add constants near the current search block in fraud_detection_precision_baseline.ipynb.
- Suggested defaults:
  - Stage 1: n_iter=80, cv=3 (fast exploration)
  - Stage 2: n_iter=120, cv=5 (accurate exploitation)
  - Keep total budget close to current 200 trials.

2. Split current param_distributions into stage1_space
- Reuse your current broad grid as stage1_space in fraud_detection_precision_baseline.ipynb.
- This preserves your current baseline behavior while making it explicitly Stage 1.

3. Run Stage 1 RandomizedSearchCV
- Replace single random_search with stage1_search in fraud_detection_precision_baseline.ipynb.
- Fit once and store stage1_search.cv_results_ and stage1_search.best_params_.

4. Build a Stage 2 focused space from Stage 1 results
- Create a helper function right below the model class in fraud_detection_precision_baseline.ipynb.
- Logic:
  - Take top-k trials from stage1_search.cv_results_ (for example, top 10 by rank_test_score).
  - For continuous-like params (learning_rate, subsample, colsample_bytree, reg_alpha, reg_lambda), use narrower ranges around top-k quantiles.
  - For discrete params (num_leaves, max_depth, min_child_samples, n_estimators), keep only values appearing in top-k plus immediate neighbors.
  - Clamp values to safe bounds (for example, learning_rate between 0.005 and 0.15).

5. Run Stage 2 RandomizedSearchCV with stricter validation
- Create stage2_search in fraud_detection_precision_baseline.ipynb.
- Use stage2_space, cv=skf (5 folds), scoring=average_precision, refit=True.
- Set best_params = stage2_search.best_params_ (fallback to stage1 best if stage2 fails).

6. Keep your OOF evaluation loop unchanged, but compare Stage 1 vs Stage 2
- In the training/evaluation block fraud_detection_precision_baseline.ipynb:
  - Print Stage 1 best CV score
  - Print Stage 2 best CV score
  - Print final OOF AP from Stage 2 params
- Add a small delta print: stage2_best_score - stage1_best_score.

7. Add overfitting guardrails
- If Stage 2 CV score improves but OOF AP does not, automatically revert to Stage 1 params.
- This check belongs near where best_params is selected in fraud_detection_precision_baseline.ipynb.

8. Make results reproducible and comparable
- Keep random_state fixed for both stages.
- Save a compact experiment summary (stage budgets, best params, CV scores, OOF AP) before submission generation in fraud_detection_precision_baseline.ipynb.

Minimal success criteria
1. Stage 1 and Stage 2 both run end-to-end without changing downstream submission code.
2. You can print both best parameter sets and both CV scores.
3. Final OOF AP is at least not worse than current single-stage baseline.