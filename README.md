# String Parser

String Parser is a Windows security-analysis tool designed to inspect dump files and the executable files referenced within them. It helps identify suspicious PE structures, embedded content, obfuscation, injection-related capabilities, persistence indicators, credential-access artifacts, and other potentially unsafe characteristics.

The application performs static analysis only: inspected files are read but never launched. Results are correlated when multiple related files are present and are presented with evidence, severity levels, and an overall risk score.

> [!IMPORTANT]
> String Parser is a triage and investigation aid. A detection does not automatically prove that a file is malicious, and a clean result does not guarantee that a file is safe. Always review findings in context.

## Requirements

- Windows 10 or Windows 11, 64-bit.
- [.NET 8 Desktop Runtime (x64)](https://dotnet.microsoft.com/en-us/download/dotnet/8.0), if it is not already installed.
- [Microsoft Edge WebView2 Runtime](https://developer.microsoft.com/en-us/microsoft-edge/webview2/), normally included with current Windows installations.
- Permission to read the selected dump and any files referenced by it.

String Parser does not require administrator privileges for normal use, an Internet connection, or a local server. The published repository contains only the protected `StringParser.exe` application and this documentation; source code is not included.

## Usage

1. Download `StringParser.exe` and keep it in a writable local folder.
2. Run `StringParser.exe`.
3. Select a supported dump file (`.txt`, `.bin`, or `.dmp`).
4. Start the scan and wait for the analysis to complete.
5. Review the discovered files, severity levels, evidence, and correlated findings.

The duration of a scan depends on the size of the dump, the number of referenced files, and the size of those files. Files that are missing, inaccessible, unsupported, or protected by insufficient permissions may not be fully analyzed.

## Checks

The engine defines 434 checks across 22 categories. Checks are assigned individual severity and weight values; some definitions may be intentionally disabled when they are outside the supported scope, redundant, or not sufficiently reliable on their own.

The table below summarizes what is covered without documenting internal signatures, thresholds, matching rules, or correlation logic.

| Group | Check area | Defined checks | Coverage summary |
|:---:|---|---:|---|
| A | DOS / PE signatures | 28 | Validity, placement, consistency, truncation, duplication, and abnormal header layout. |
| B | COFF file header | 16 | Architecture, section count, timestamp, symbol information, and contradictory file characteristics. |
| C | Optional header | 24 | PE format consistency, entry point, image layout, alignment, permissions, and declared image size. |
| D | Data directories | 16 | Directory counts, ranges, overlaps, invalid sizes, and references outside the file. |
| E | PE sections | 32 | Section names, permissions, entropy, dimensions, ordering, overlap, gaps, and padding anomalies. |
| F | Overlay / polyglot content | 18 | Appended data, compressed or encrypted-looking content, embedded formats, and mixed-file structures. |
| G | Import table | 24 | Import integrity, unusual counts, ordinal use, native APIs, and capability-related API groups. |
| H | Dynamic API resolution | 14 | Runtime library loading, dynamic symbol resolution, manual export lookup, and API-hashing indicators. |
| I | Export table / DLL | 14 | Export validity, unusual naming or forwarding, proxy-DLL profiles, and DLL inconsistencies. |
| J | Resources / version information | 20 | Embedded resources, metadata consistency, suspicious identity fields, manifests, and elevated privileges. |
| K | TLS / relocations | 12 | TLS directory and callbacks, callback placement, relocation integrity, and ASLR consistency. |
| L | Signature / debug / PDB | 18 | Digital-signature state, certificate structure, signer consistency, debug data, and development paths. |
| M | Packing / obfuscation | 20 | Known protector profiles, entropy, sparse readable content, unusual stubs, and padding patterns. |
| N | Embedded payloads | 16 | Embedded PE files, archives, scripts, partially hidden executables, and high-entropy binary blobs. |
| O | Memory manipulation / injection | 24 | Process, memory, and thread manipulation capabilities associated with common injection techniques. |
| P | Persistence | 18 | Startup, registry, service, scheduled-task, WMI, COM, browser, and DLL-loading persistence indicators. |
| Q | Command execution / LOLBins | 20 | Shells, scripting engines, encoded commands, download utilities, and commonly abused Windows tools. |
| R | Networking | 20 | Network APIs, hard-coded endpoints, user agents, webhooks, anonymization, and proxy-related artifacts. |
| S | Credentials / input / capture | 20 | Credential stores, browser data, DPAPI, keyboard or clipboard access, dumping, and screen capture. |
| T | Anti-analysis | 20 | Debugger, timing, analysis-tool, virtual-machine, sandbox, monitoring, and environment indicators. |
| U | .NET / managed PE | 20 | CLR structure, managed metadata, reflection, dynamic code, P/Invoke, and managed obfuscation profiles. |
| V | IOC / composite rules | 20 | Known indicators and higher-confidence combinations of multiple suspicious characteristics. |
| **Total** | **22 categories** | **434** | **Structural checks, capability indicators, metadata review, and contextual correlation.** |

## Understanding results

Each finding includes a stable check identifier, its category, severity, and supporting evidence. The final risk level is based on the combined context of the findings rather than on a single generic string or API name.

Severity levels range from informational observations to critical findings. Informational and low-severity entries can describe legitimate software characteristics; higher severities should be investigated first but still require human review.

## Privacy and safety

- Analysis is performed locally.
- The tool does not upload dumps, inspected files, or results.
- Inspected executables are not launched.
- No Internet connection is required during a scan.

Only analyze files and dumps that you are authorized to access.
