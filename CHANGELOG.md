# Changelog

## Unreleased

### Fixed

- The project list no longer drops its last page. `GetProjects` in `OpenProjectAPIModul` stopped the page loop with `>=` after the page number had already been incremented, so the last page returned by `GET /api/v3/projects` was never written to the Projects sheet. A project that landed on that page was then reported as `Project not found or no access. Abort.`, even when the identifier from the URL was correct and the API token could see the project. The loop now uses `>`, the same end condition as the work-package download. This only changes behavior when the user can see more projects than one page (the API default page size is 20).

  Reported earlier:

  - [opf/OpenProjectExcel#69](https://github.com/opf/OpenProjectExcel/issues/69) — project list stops before the last page; the `>=` condition in `GetProjects` is identified, and `>` is the proposed fix
  - [opf/OpenProjectExcel#57](https://github.com/opf/OpenProjectExcel/issues/57) — `Project not found or no access. Abort.` for some projects and not for others
  - [ES-72](https://community.openproject.org/projects/ES/work_packages/47347) — same error; later note that the last project page is not loaded because the loop ends on `>=`
  - [ES-71](https://community.openproject.org/projects/ES/work_packages/47346) — closed as a duplicate of ES-72
  - [ES-73](https://community.openproject.org/projects/ES/work_packages/48124) — the error for a few projects while the rest load; same last-page diagnosis
  - [ES-70](https://community.openproject.org/work_packages/46173) — the error when the user can see on the order of 50–100 projects
