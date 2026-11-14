# Public release catalog

This repository is the public distribution catalog for LLM Assistants. Keep private application source/history in the development repository. Only commit public documentation, license notices and release metadata. Attach explicitly reviewed installer/portable/voice-tool packages and SHA256SUMS to releases. Never publish voice references, model weights, user profiles, tokens, caches or raw diagnostics.

Keep installation instructions understandable without development knowledge; document technical contracts separately. Preserve platform/architecture and prerelease limits. Never claim Windows acceptance from a build on Mac. Do not silently promote a prerelease to stable.

Use annotated SemVer tags matching package versions, Conventional Commits and the configured human author. No agent branding in Git/GitHub subjects. Stage uploads in a draft, verify bytes/checksums and publish only within user authorization. Verify the public API without authentication after publication.
