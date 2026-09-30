# Mermaid diagram notes

The `.mmd` files in this folder are designed for Mermaid-compatible viewers and VS Code Mermaid extensions.

## Zoom / pan

Zoom and pan are viewer/extension features rather than a Mermaid syntax command. The diagrams are intentionally split into smaller files and use Mermaid's flowchart spacing settings so they remain comfortable to zoom and navigate.

## Color system

- 🟢 Green = MongoDB / completed / output concepts
- 🔵 Blue = setup / query / filtering
- 🟣 Purple = structure / aggregation
- 🟠 Orange = operators / calculations
- 🩷 Pink = complex document concepts / logical flow
- 🔴 Red = delete / important operator
- ⚫ Slate = tools / inputs

Each diagram contains its own `%%{init: ...}%%` configuration and `classDef` definitions, so it can be copied and rendered independently.
