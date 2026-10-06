> **Retired 2026-10-06, archived read-only.** Its documents were byte-identical to the
> [`catalog`](https://github.com/frompt-org/catalog), and its stated job — being what the docs and
> the conformance suite point at — was already done by [`protocol`](https://github.com/frompt-org/protocol),
> which publishes the same frompts as a signed catalog of its own. Kept as the record, not maintained.
> Its manifest expires 2026-10-09 and will not be renewed: every verify against it refuses after that.

# reference — the reference catalog

**A foreign prompt is a prompt acquired from somewhere else — usually a URL — and adopted, on purpose, by an agent that did not write it.** It is prompt injection you chose: the pilot names one document, the document declares what it intends, the agent announces that it started, and it ends.

**You are in a catalog** — a signed manifest plus the documents it lists. A catalog is a shape, not a privilege: anyone who can serve static files can publish one, and no catalog is more official than another. This is simply the first.

The protocol, the tools and the spec are not here; they are in [`frompt-org/fpa`](https://github.com/frompt-org/fpa). Start at [`README`](https://github.com/frompt-org/frompt#readme) if none of the above was familiar.

**This catalog is private.** That costs nothing: the protocol's trust comes from a signed
manifest and a locally held key, not from being reachable anonymously, so `gh:` fetches it
through the credential you already have and the digest still binds. Publishing is a decision
about audience.

```
index.json                        the signed manifest; a client fetches this first
index.json.sig
INDEX.md                          the same thing, for people
publisher.pub                     the key — read it here once, obtain it elsewhere (below)
prompts/<id>/<version>.frompt.md  the documents, immutable
```

## Adopting from here

A pilot reads a prompt, then sends the phrase from its final `## Consent` section together with the URL. Nothing is installed.

An unattended client verifies instead of reading:

```bash
fp-verify pr-review@2.0.0 --from gh:frompt-org/reference@main    # private: the GitHub API
fp-resolve pr-review@latest --lock                            # against a committed fpa.lock
```

A `raw.githubusercontent.com` base works the same way, and will 404 until this repository is
public — the transport changes, the digest does not.

Tools and protocol: [`fpa`](https://github.com/frompt-org/fpa) —
the spec is `FPA.md`, and `VISION.md` explains what a catalog is for and who else is meant to
publish one.

## The one thing to get right

**Do not take `publisher.pub` from this repository at verification time.** A key fetched from the same host as the signature it validates proves only that the two agree with each other. Copy it once, out of band — into your plugin, your dotfiles, your MDM — and pin it there.

## Publishing here

Versions are immutable: `prompts/pr-review/2.0.0.frompt.md` never changes its bytes. A change is a new version, because every lock file and every confirmation phrase in the world is bound to the digest of what was published.

```bash
fp-publish ~/frompt/reference     # from the protocol repo: stage the documents
fp-index                           # rebuild INDEX.md and index.json
fp-sign                            # sign the manifest, key from outside the repo
git add -A && git commit && git push
```

Commit the prompts, the manifest and its signature together. Split across commits, a client can fetch a manifest that does not match what is served.

## Reading further

| Question | Document |
|---|---|
| What is a foreign prompt, and how do I try one? | [`fpa/README.md`](https://github.com/frompt-org/frompt#readme) |
| What must an agent actually do? | [`FPA.md`](https://github.com/frompt-org/fpa/blob/main/FPA.md) — normative |
| Why does this project exist, and who else is meant to publish? | [`VISION.md`](https://github.com/frompt-org/frompt/blob/main/VISION.md) |
| What am I trusting, and what am I not? | [`SECURITY.md`](https://github.com/frompt-org/fpa/blob/main/SECURITY.md) |
| What is published here, with digests? | [`INDEX.md`](INDEX.md) |
| What do the words mean? | [`TERMINOLOGY.md`](https://github.com/frompt-org/fpa/blob/main/TERMINOLOGY.md) |
