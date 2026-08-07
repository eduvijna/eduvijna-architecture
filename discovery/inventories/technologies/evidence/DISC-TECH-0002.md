# Evidence Summary — DISC-TECH-0002

**Technology:** `Expo`
**Source repository:** `Eduvijna`
**Discovered at:** 2026-08-07T16:00:00Z
**Confidence:** HIGH
**Category:** framework

## Facts (evidenced)

| Field | Value |
|-------|-------|
| technology_id | TECH-expo-eduvijna |
| name | Expo |
| category | framework |
| version_observed | ~54.0.35 |
| usage_context | Mobile application framework; package.json main entry is node_modules/expo/AppEntry.js; expo scripts present. |
| support_status | UNKNOWN |

## Evidence table

| Type | Location | Description |
|------|----------|-------------|
| package-json | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna__package.json.txt` | dependencies.expo is ~54.0.35; scripts start/android/ios/web invoke expo. |
| other | `discovery/inventories/repositories/evidence/_raw/eduvijna__Eduvijna-root.json` | Root listing evidences app.json and eas.json alongside package.json. |

## Unknowns

- `technology.support_status`
