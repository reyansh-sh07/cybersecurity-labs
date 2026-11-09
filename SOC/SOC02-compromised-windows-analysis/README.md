# SOC02 — Compromised Windows Analysis

**Platform:** TryHackMe  
**Room:** [Compromised Windows Analysis](https://tryhackme.com/room/compromisedwindowsanalysis)  
**Status:** Completed  
**Category:** Digital Forensics and Incident Response (DFIR)

## Overview

This guided Windows investigation focuses on identifying attacker activity on a compromised workstation, examining persistence and execution artefacts, analysing Windows event logs, and reconstructing an incident timeline.

I completed the room as a beginner DFIR exercise. The guided workflow introduced how an analyst combines multiple forensic artefacts rather than relying on a single alert or log source.

## Investigation workflow

The guided room introduced the following tools and evidence sources:

1. **Timeline Explorer** — reviewing parsed forensic artefacts and building a chronological view.
2. **Scheduled Tasks** — checking task configuration and execution behaviour for potential persistence.
3. **Windows shortcut (LNK) artefacts** — examining evidence of recently accessed files.
4. **LECmd** — parsing LNK artefacts into structured output.
5. **Amcache** — reviewing application metadata associated with executable files.
6. **Prefetch** — examining program-execution artefacts.
7. **Windows Event Viewer** — reviewing relevant Windows event channels, including Remote Desktop and Windows Defender operational logs.
8. **Timeline correlation** — combining timestamps and artefacts to reconstruct the sequence of activity.

## What I learned

- Windows investigations rely on correlating multiple artefact sources.
- Scheduled Tasks can be abused for persistence; task actions, paths, timestamps, and execution context matter.
- LNK files can preserve evidence of recently accessed files, even when the original file is later unavailable.
- Amcache and Prefetch provide different forms of application and execution evidence and should not be interpreted in isolation.
- Windows Event Logs provide context about remote access and security-control activity.
- A timeline helps connect separate observations into a defensible incident narrative.
- A strong investigation distinguishes observed facts from analyst interpretation.

## Tools and artefacts

- Timeline Explorer
- Eric Zimmerman's LECmd
- Amcache Parser
- PECmd / Prefetch artefacts
- Windows Task Scheduler
- Windows Event Viewer
- Remote Desktop and Windows Defender operational logs

## Outcome

**Completed.** I completed the TryHackMe room, including its final chronological-order task.

The room's investigation scenario covers suspicious remote access, security-control tampering, dropped files, executable activity, and scheduled-task persistence. This report focuses on the forensic workflow and learning outcomes rather than reproducing room answers, flags, or challenge-specific indicators.

## Evidence

The room-completion screenshot belongs in this directory:

`evidence/01-room-completed.png`

## Scope and assistance disclosure

This was a guided TryHackMe exercise using the platform-provided lab machine and forensic artefacts. It was not an investigation of a real organisation or production endpoint. I used the room's guidance and AI assistance to understand unfamiliar tools and concepts while completing the practical investigation.
