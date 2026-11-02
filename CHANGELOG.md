# Changelog

## 4.0.0 (31 Oct 2026): first public release

- **Scoring:** PQM value functions. Full points at or better than the achievable ideal, linear to 25% of the points at the threshold, 0 beyond it.
- **Benchmarks:** bin medians of 367 saturated atlas plans (MASTER v13), by volume and shape. Brain dose is also by scheme and location.
- **Conformity index:** the user chooses CI_RTOG or the Paddick CI; only the chosen index is scored.
- **Zones:**

  | Zone | Rule |
  |---|---|
  | Constraint failed | Brain or OAR limit exceeded, D99 < 90%, or HI > 1.2 |
  | Re-optimise | vPQM < 75 |
  | Review | vPQM 75–85 |
  | Meets benchmark | vPQM > 85 |

- **Input checks:**
  - volume outside the atlas range;
  - Paddick CI × CI_RTOG > 1.02 (inconsistent indices);
  - Q_RTOG > D99.
- **Copy result as text**, for QA records.
- **Regression test** against the Python reference implementation (`tests/`).
