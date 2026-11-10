# PRAXO-CORE for Python

Dedicated repository for the Python implementation of PRAXO-CORE: an
embeddable, MIT-licensed agent execution kernel with policy controls,
human-in-the-loop approval, checkpoints, events, and tool calling.

- Package: `praxo-core` on PyPI
- Documentation: https://axisvela.github.io/praxo-core/languages/python/
- Consumer starter: https://github.com/axisvela/praxo-core/tree/main/examples/consumer-python
- Canonical source: https://github.com/axisvela/praxo-core/tree/main/language/python
- Release index: https://axisvela.github.io/praxo-core/languages/registry-status/

This repository is a discovery mirror of the canonical Python source. The
monorepo remains the source of truth and the release workflow source. The
shared conformance vectors are included under `spec/`, so the implementation can
be built and tested here.

## Install

```sh
pip install praxo-core==0.3.2
```

## Verify

From `language/python`:

```sh
python conformance/run.py
python -m unittest discover -s tests -v
python api-snapshot.py --check
```

## Scope

The Python Core targets the eleven shared vector groups and the agent loop.
It is a native implementation, not a binding over the Go reference. See the
canonical documentation for declared behavior, limitations, and evidence.

PRAXO-CORE is not a hosted model, workflow engine, identity provider, or safety
controller. Do not infer exactly-once execution, complete sandbox isolation,
production security certification, live-provider behavior, or physical safety
from this package.

MIT licensed.
