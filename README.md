# PRAXO-CORE for Python

Dedicated repository for the Python implementation of PRAXO-CORE: an
embeddable, MIT-licensed agent execution kernel with policy controls,
human-in-the-loop approval, checkpoints, events, and tool calling.

- Package: `praxo-core` (`0.7.0`)
- Registry: PyPI
- Install: `pip install praxo-core==0.7.0`
- Documentation: https://axisvela.github.io/praxo-core/languages/python/
- Examples: https://github.com/axisvela/praxo-core-python/tree/main/language/python/examples
- Mirror repository: https://github.com/axisvela/praxo-core-python
- Canonical source: https://github.com/axisvela/praxo-core/tree/main/language/python
- Release index: https://axisvela.github.io/praxo-core/languages/registry-status/

This repository is a discovery mirror of the canonical Python source.
The monorepo remains the source of truth and the release workflow source. Shared
conformance vectors are included under `spec/`, so the implementation can be
built and tested here.

## Verify

Build and test instructions are in `language/python/README.md`. The
monorepo CI runs the language's conformance, API snapshot, and native tests.

PRAXO-CORE is not a hosted model, workflow engine, identity provider, or safety
controller. Do not infer exactly-once execution, complete sandbox isolation,
production security certification, live-provider behavior, or physical safety
from this package.

MIT licensed.
