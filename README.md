# vPQM: volume-stratified plan quality metric for brain SRS/SRT

**A plan quality assurance and assessment tool. Not for clinical decision-making.**

The vPQM compares a finished intracranial stereotactic plan with what fully optimised plans of the same target volume, shape and location achieved in a reference atlas of 367 saturated VMAT plans.

- **Safety and coverage** (normal-brain dose, OAR dose, D99) are scored against absolute limits.
- **Plan quality** (conformity, gradient, Q_RTOG) is scored against the achievable benchmark for the plan's geometry.

Users choose the conformity index:

- **CI_RTOG** = PIV / TV
- **Paddick CI** = TV_PIV² / (TV × PIV), recommended by ICRU Report 91

**Live tool:** `https://<your-username>.github.io/<repository-name>/` (after GitHub Pages is enabled; see below)

---

## Quick start

1. Open `index.html` in any modern browser. Everything runs locally: no installation, no internet connection, and no data sent or stored.
2. Choose the conformity index your department reports.
3. Enter the scheme (SRS, 3 or 5 fractions), PTV volume, shape (regular/irregular), location (central/peripheral) and the plan metrics.
4. Read the score, the zone and the component breakdown. Use **Copy result as text** to attach the check to your QA record.

| Zone | Meaning |
|---|---|
| Constraint failed (red) | A brain or OAR limit is exceeded, D99 < 90%, or HI > 1.2. |
| < 75, re-optimise | Re-optimisation is likely to improve the plan. |
| 75–85, review | Acceptable, but one or more components lie below the achievable benchmark. |
| > 85, meets benchmark | The plan reaches the achievable benchmark for its geometry. |

## Publish with GitHub Pages

1. Create a new public repository and upload the contents of this folder, keeping the folder structure.
2. Go to **Settings → Pages → Build and deployment**, set **Source: Deploy from a branch**, choose **Branch: main** and the **/ (root)** folder, then **Save**.
3. After about a minute the tool is live at `https://<your-username>.github.io/<repository-name>/`.

The empty `.nojekyll` file tells GitHub Pages to serve the files as they are.

## Repository contents

| Path | Content |
|---|---|
| `index.html` | The complete application: a single file with no external dependencies. |
| `docs/METHODS.md` | How the score is built, with the benchmark tables and references. |
| `data/benchmarks.json` | The benchmarks embedded in the app, in machine-readable form. |
| `data/benchmarks_quality.csv` | Ideal and threshold of CI_RTOG, Paddick CI, GI and Q_RTOG by shape and volume bin. |
| `data/benchmarks_brain.csv` | Achievable normal-brain dose by scheme, location, shape and volume bin. |
| `validation/` | Template and instructions for multi-institutional validation. |
| `tests/` | Synthetic test cases (not patient data) and a regression test: `node tests/run_tests.js`. |
| `.github/ISSUE_TEMPLATE/` | Forms for validation submissions and bug reports. |
| `DISCLAIMER.md`, `LICENSE`, `CITATION.cff`, `CHANGELOG.md` | Terms of use, licence, how to cite, version history. |

## Intended use and limitations

- The vPQM is a **quality-assurance and research aid**. It is not a medical device and not a basis for clinical decisions. Plan acceptance remains the responsibility of qualified medical physicists and radiation oncologists under local protocols.
- **Single-institution benchmarks.** They were derived on one planning system and linac: Monaco, Elekta Versa HD, 6 MV FFF VMAT, three-arc class solution. Other systems, techniques or prescription conventions may achieve different values.
- **It scores only what is entered.** It cannot assess re-irradiation or cumulative dose, OARs that are not entered, delineation, dose-calculation accuracy or deliverability.
- **Development-cohort performance.** In 41 clinical plans (13 returned for re-optimisation), the AUC was about 0.91 with either conformity index, and no returned plan scored above 85. The 75/85 boundaries were set on this cohort and are being tested prospectively.

## Multi-institutional validation

We invite departments to score their own plans and share anonymised records so that the benchmarks and cut-offs can be validated across institutions, planning systems and techniques. See [`validation/README.md`](validation/README.md). Submit through a GitHub Issue ("Validation submission") or by e-mail to **[add contact e-mail]**. Do not upload identifiable patient data.

## Development

The scoring engine is the `score()` function in `index.html`. After any change, run the regression test (Node.js ≥ 18):

```
node tests/run_tests.js
```

The expected values come from the independent Python reference implementation used for the publication.

## Citation

If you use the tool, please cite the publication (see `CITATION.cff`):

Eid AA, et al. *Volume-stratified benchmarks of achievable plan quality and a volume-adaptive plan quality metric (vPQM) for stereotactic radiosurgery and radiotherapy.* [journal, year, DOI to add]

Designed by Ali Eid. Version vPQM v4 (benchmark atlas MASTER v13, 31 Oct 2026).
