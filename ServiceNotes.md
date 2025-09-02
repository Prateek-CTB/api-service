## Overview

This service emulates a compact production-style application with a small set of routes for demos and testing. The notes below summarize a few implementation caveats at a high level.

## Authentication & Authorization

- Environment defaults: Administrative login can be configured via environment; development defaults are provided.
- Claim-based checks: Some authorization decisions rely on token claims.

## Data Handling

- SQL queries: Some queries use string interpolation for simplicity.
- Output encoding: User content may render into HTML with minimal escaping in places.
- Debug surfaces: Operational details can be toggled via debug controls.

## External Interactions

- Remote fetch: Server-side URL retrieval includes basic scheme checks and timeouts.

## Logic & Concurrency

- Transfer flow: Balance updates are implemented as simple read/modify/write with simulated delay.

Use only in isolated environments. Do not expose to the public internet.

