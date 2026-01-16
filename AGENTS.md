# Repository Guidelines

## Project Structure & Module Organization

- Game assets live under `Assets/!_ProjectMain`: `Scenes/Prototype.unity` (entry scene), `Art`, `Audio`, `Prefabs`, and `Scripts` (add gameplay code here). Reserve `Assets/!_ThirdParty` for vendor content so it is easy to diff and upgrade.
- Project configuration is in `ProjectSettings/` and `Packages/`; do not edit `Library/` or `Logs/` by hand.
- Keep input maps in `Assets/InputSystem_Actions.inputactions`; update and regenerate wrappers through the Unity Input System window to avoid merge issues.

## Build, Test, and Development Commands

- Open the project with Unity 6000.3.2f1 via Unity Hub; match the editor version to avoid serialization churn.
- Local playtest: open `Prototype.unity`, press Play in the editor, and watch the Console for warnings.
- Example CLI launches (adjust editor path for your OS):

  ```bash
  /path/to/Unity-6000.3.2f1 -projectPath . -logFile Logs/editor.log
  ```

- For CI-style builds, add a BuildPipeline script first; then run headless builds with `-quit -batchmode -nographics` and the appropriate `-buildTarget`/`-build*Player` arguments.

## Coding Style & Naming Conventions

- C# scripts: 4-space indentation, PascalCase types, camelCase fields, and file names matching the public class. Prefer `[SerializeField] private` over public fields; mark read-only data with `readonly`.
- Prefabs and scenes: prefix by feature when helpful (e.g., `UI_`, `FX_`); keep one primary scene (`Prototype`) focused and load additively for feature scenes.
- Avoid hard-coded asset paths; use references on prefabs/ScriptableObjects for configurability.

## Testing Guidelines

- Use the Unity Test Framework; place edit-mode tests in `Assets/Tests/EditMode` and play-mode tests in `Assets/Tests/PlayMode`.
- Suggested CLI once tests exist:

  ```bash
  /path/to/Unity-6000.3.2f1 -projectPath . -quit -batchmode -runTests -testPlatform PlayMode -logFile Logs/tests.log
  ```

- Aim for coverage on core loops (resource accrual, timers, save/load) and guard against regressions with deterministic test data.

## Commit & Pull Request Guidelines

- Commit messages: short, imperative subject lines (e.g., `Add idle income ticker`). Current history is minimal; adopt Conventional Commits if you expand automation.
- Include PR summaries of changes, playtest notes, and any known limitations. Link to issues/tasks and attach editor or play-mode screenshots/gifs when visuals change.
- Before submitting, ensure scenes and prefabs are saved, Console warnings are addressed, and no unintended asset re-import noise is present in `Assets/` or `ProjectSettings/`.

## Version Control & Asset Hygiene

- Keep `.meta` files under version control; avoid committing `Library/`, `Logs/`, or build outputs.
- When adding large binaries, prefer compression or split packs; document any new third-party assets in `Assets/!_ThirdParty` with their license and source.
