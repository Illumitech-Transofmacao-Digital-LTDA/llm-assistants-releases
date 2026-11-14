# Public release catalog

This repository is the public distribution catalog for LLM Assistants. Keep private application source/history in the development repository. Only commit public documentation, license notices, release metadata and the single approved reference specified below. Attach explicitly reviewed installer/portable/voice-tool packages and SHA256SUMS to releases. The user explicitly authorized publishing the approved Capri reference on 2026-11-14. Only assets/voices/capri/capri-voice-reference.wav with SHA-256 8ca29cad94fccac2c081a57bc4e74ecf16c4a479f2ef8c389f7829db1d84cb26 may be included as the public voice reference. Never publish other recordings, model weights, user profiles, tokens, caches or raw diagnostics.

Keep installation instructions understandable without development knowledge; document technical contracts separately. Preserve platform/architecture and prerelease limits. Never claim Windows acceptance from a build on Mac. Do not silently promote a prerelease to stable.

Use annotated SemVer tags matching package versions, Conventional Commits and the configured human author. No agent branding in Git/GitHub subjects. Stage uploads in a draft, verify bytes/checksums and publish only within user authorization. Verify the public API without authentication after publication.
