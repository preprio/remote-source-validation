# Release instructions

When preparing a release:

1. For a non-breaking change, increment the patch version (`v1.0.17` → `v1.0.18`). Use the latest release version to determine the next version; the private `package.json` version is not the release version.
2. Update `README.md` if CLI usage or response examples change.
3. Prepend an entry to `CHANGELOG.md`, preserving the existing format. Keep the schema date, set the revision number to the release patch version (`v1.0.18` → revision #18), use the release date, and add a short bullet describing the changes. Only describe the release as non-breaking if it is compatible with previously valid responses.

   ```markdown
   ## 2026-03-05 revision #18

   This is a non-breaking revision of the existing `2026-03-05` version. Released on 15 October 2026.

   - Short summary of the changes.
   ```

   The revision number and release date above are examples; derive them for each release.

4. Output a Conventional Commits message describing the changes, such as `feat(): validate optional filter values` or `fix(): correct response validation`.
