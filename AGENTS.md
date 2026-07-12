# Repository Guidance

## Scope

- Preserve the original MATLAB experiment behavior unless a change is explicitly requested.
- Keep the existing experiment directory names because scripts rely on relative paths.
- Document any required dataset that cannot be committed to the repository.

## Validation

- Inspect changed MATLAB files for valid relative data and function paths.
- Run experiments from their own experiment directory.
- Record MATLAB version, toolbox availability, random seed, and parameter changes when reporting reproduced results.

## Documentation

- Keep README instructions aligned with files actually present in the repository.
- Add the final paper citation only after its bibliographic details are confirmed.
- Do not imply an open-source license until the copyright holders choose one.

## Continuous Improvement

- Record reusable findings in `.learnings/LEARNINGS.md`.
- Record failed commands, environment issues, and reproduction blockers in `.learnings/ERRORS.md`.
- Record requested missing capabilities in `.learnings/FEATURE_REQUESTS.md`.
