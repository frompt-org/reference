# The reference catalog

Foreign prompts, published. A **catalog** is a shape, not a privilege — anyone who can serve static files can publish one, and no catalog is more official than another. This is simply the first.

```
index.json                        the signed manifest; a client fetches this first
index.json.sig
INDEX.md                          the same thing, for people
publisher.pub                     the key — read it here once, obtain it elsewhere (below)
prompts/<id>/<version>.prompt.md  the documents, immutable
```

## Adopting from here

A pilot reads a prompt, then sends the phrase from its final `## Consent` section together with the URL. Nothing is installed.

An unattended client verifies instead of reading:

```bash
fp-verify pr-review@2.0.0 --from https://raw.githubusercontent.com/f-prompts/prompts/main
fp-resolve pr-review@latest --lock          # against a committed fpa.lock
```

Tools and protocol: [`foreign-prompts`](https://github.com/agent-realm/foreign-prompts).

## The one thing to get right

**Do not take `publisher.pub` from this repository at verification time.** A key fetched from the same host as the signature it validates proves only that the two agree with each other. Copy it once, out of band — into your plugin, your dotfiles, your MDM — and pin it there.

## Publishing here

Versions are immutable: `prompts/pr-review/2.0.0.prompt.md` never changes its bytes. A change is a new version, because every lock file and every confirmation phrase in the world is bound to the digest of what was published.

```bash
fp-publish ~/f-prompts/prompts     # from the protocol repo: stage the documents
fp-index                           # rebuild INDEX.md and index.json
fp-sign                            # sign the manifest, key from outside the repo
git add -A && git commit && git push
```

Commit the prompts, the manifest and its signature together. Split across commits, a client can fetch a manifest that does not match what is served.
