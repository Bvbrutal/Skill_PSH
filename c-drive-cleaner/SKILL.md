---
name: c-drive-cleaner
description: "Scan Windows C drive for cache files, temp files, orphaned software leftovers, large files, and long-unused files. Generate a cleanup report with risk levels for user review. Use this skill when the user mentions C drive space issues, disk cleanup, cache cleanup, temp file cleanup, C drive full, free up space, clean junk files, or disk space analysis. Trigger even for questions like Why is my C drive full or How to clean C drive."
---

# Windows C Drive Cleanup Assistant

Scan C drive for cleanable cache/temp/remnant/large/long-unused files with risk assessment and deletion rationale. Generate report for user decision.

## Core Principles

- **Report only, never delete**: This skill only generates cleanup reports, never directly deletes any files
- **Every item needs rationale**: Each suggested cleanup item must explain why it can be deleted
- **Transparent risk levels**: All items labeled with risk level (Low/Med/High) and deletion impact
- **User is the final decision maker**: Only execute cleanup after explicit user confirmation

## Workflow

### Step 1: Quick Scan

Get C drive space overview.

### Step 2: Category Scan

Reference references/cache_directories.md. Record: path, size, risk, rationale, impact.
Categories: 1.System Temp(Low) 2.Browser Cache(Low) 3.WinUpdate(Low-Med) 4.DevCache(Low-Med) 5.AppCache(Low-High) 6.Orphans(Med) 7.LargeFiles(Case) 8.SpecialFiles(High)

### Step 3: Batch Scan

Run scripts/c-drive-scan.ps1. Modes: quick/deep/orphan.

### Step 4: Report

Template: Scan Time, Drive Space, Reclaimable, Table, Risk Legend, Top 5.

### Step 5: User Judgment

Confirm/Skip/Info. Execute by risk.

## Risk Guide

|Dim|Low|Med|High|
|---|---|---|---|
|Data|No loss|Re-download|Data loss|
|Impact|None|Minor|Unavailable|
|Recovery|Auto|Manual|Irrecoverable|

## Notes

1. Never delete pagefile.sys/swapfile.sys
2. Use powercfg for hiberfil.sys
3. Use Dism for WinSxS
