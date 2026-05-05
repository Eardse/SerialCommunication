# Copilot instructions for SerialCommunication

This file helps Copilot sessions understand how to build, run, and change this repository.

---

## Build, run, test, and lint commands

- Open the solution: SerialCommunication.slnx (root) in Visual Studio 2017/2019/2022 and Build.

- MSBuild (solution):
  msbuild "SerialCommunication.slnx" /p:Configuration=Debug
  msbuild "SerialCommunication.slnx" /p:Configuration=Release

- MSBuild (single project):
  msbuild "SerialCommunication\SerialCommunication.csproj" /p:Configuration=Debug

- Restore packages (if applicable):
  nuget restore "SerialCommunication.slnx"
  (For SDK-style projects use `dotnet restore`, but this repo is .NET Framework WinForms.)

- Run locally (Visual Studio): Start debugging (F5) or run without debugging (Ctrl+F5).
- Run executable directly: SerialCommunication\bin\Debug\SerialCommunication.exe (built output).
  Note: Accessing serial devices may require elevated permissions on some Windows setups; run Visual Studio or the EXE as Administrator if ports are unavailable.

- Tests: There are no automated tests or test runner configured in this repository. No single-test command is available.
- Linting/formatting: No linter or formatter is configured. Follow existing code style when making changes.

Notes: This is a .NET Framework 4.7.2 WinForms project—prefer MSBuild/Visual Studio over dotnet CLI unless migrating to SDK-style projects.

---

## High-level architecture (big-picture)

- Type: Windows Forms desktop application (WinForms) targeting .NET Framework 4.7.2.
- Entry point: Program.Main (SerialCommunication\SerialCommunication\Program.cs) — launches Form1.
- UI: Single main form implemented in Form1 (Form1.cs + Form1.Designer.cs + Form1.resx). Designer-generated files hold control definitions; runtime logic belongs in Form1.cs or helper classes.
- Serial communications: Uses System.IO.Ports.SerialPort for port enumeration and intended I/O. Enumeration occurs in Form1_Load and when the port ComboBox is opened.
- Resources: Images and string resources are under Properties/Resources.resx and Resources/; user defaults are in Properties/Settings.settings.
- Project layout: solution at repository root, project folder SerialCommunication/. Build artifacts live in bin/ and obj/.

---

## Key repository conventions and notes for Copilot

- Designer files: Do not edit Form1.Designer.cs or generated resource/designer files by hand. Make UI edits via the WinForms Designer; if code edits are required, modify Form1.cs and keep the designer file untouched.

- Event-handler pattern: Event handlers in Form1.cs are intentionally thin. When adding behavior, extract logic into private methods or new classes and call them from handlers to keep UI code maintainable.

- Serial-port handling:
  - Ports are enumerated with SerialPort.GetPortNames().Distinct().ToArray(). Follow this pattern when refreshing port lists.
  - Default baud rate used in the UI is "115200". Preserve or expose this via Settings if adding configuration.
  - Current code uses empty catch blocks in UI events. When improving error handling, prefer logging (e.g., Debug/Trace) and non-blocking user feedback (MessageBox) rather than throwing unhandled exceptions.

- Resources & Settings:
  - Reference images via Properties.Resources.<Name>.
  - Store user defaults in Properties/Settings.settings so Settings.Designer.cs remains the single source of truth for defaults.

- Package management: Use NuGet for dependencies. Commit package references in the .csproj (PackageReference) or packages.config as appropriate.

- Ignored/generated files: Keep .vs/, bin/, obj/ and other generated artifacts out of commits. Use the existing .gitignore as the source of truth.

---

## Existing AI/assistant files scanned

- Scanned for other assistant configs and none were found: no CLAUDE.md, .cursorrules, AGENTS.md, .windsurfrules, CONVENTIONS.md, or AIDER_CONVENTIONS.md.

---

## Quick pointers for future Copilot sessions

- Target guidance to WinForms idioms and .NET Framework 4.7.2 compatibility.
- Prefer changes in Form1.cs or new helper classes; avoid touching auto-generated designer/resource files.
- When suggesting build commands, include msbuild and nuget restore steps and mention that elevated permissions may be required for COM port access.

---

(End of copilot-instructions.md)
