# llms.txt

`docs.opencastor.com/llms.txt` provides a machine-readable index of this documentation for AI assistants and language models.

## Format

Following the [llmstxt.org](https://llmstxt.org) convention:

```
# OpenCastor Documentation

> OpenCastor is an open-source runtime layer for AI-controlled robots.
> It handles authentication, safety invariants, telemetry, harness management,
> and fleet coordination via the RCAN protocol.

## Getting Started
- [Installation](https://docs.opencastor.com/getting-started/installation/)
- [Quickstart](https://docs.opencastor.com/getting-started/quickstart/)
- [Configuration](https://docs.opencastor.com/getting-started/configuration/)

## Runtime
- [Overview](https://docs.opencastor.com/runtime/overview/)
- [Agent Harness](https://docs.opencastor.com/runtime/harness/)
- [Safety & P66](https://docs.opencastor.com/runtime/safety/)
- [Contribute](https://docs.opencastor.com/runtime/contribute/)
- [CLI Reference](https://docs.opencastor.com/runtime/cli/)
- [REST API](https://docs.opencastor.com/runtime/api/)

## RCAN Protocol
- [Overview](https://docs.opencastor.com/rcan/overview/)
- [Message Types](https://docs.opencastor.com/rcan/message-types/)
- [Scopes](https://docs.opencastor.com/rcan/scopes/)

## Research
- [Harness Research](https://docs.opencastor.com/research/overview/)
- [OHB-1 Benchmark](https://docs.opencastor.com/research/ohb1-benchmark/)
- [Search Space](https://docs.opencastor.com/research/search-space/)

## Optional
- [Changelog](https://docs.opencastor.com/reference/changelog/)
- [Versioning](https://docs.opencastor.com/reference/versioning/)
```

## Why llms.txt?

When an AI assistant is asked about OpenCastor, it can fetch `llms.txt` to get a structured overview of all documentation URLs — much faster than crawling the site. MkDocs makes it easy to serve this as a static file.

The file is served at: `https://docs.opencastor.com/llms.txt`
