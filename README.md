# Dark factory

One prompt that builds and runs a dark factory for your repository. Intent goes in, shipped software comes out, and no human writes or reviews the code.

Grok Bots run the factory: specs, scenarios, parallel builders, digital twins of your third-party services, verification, merges, progressive deploys, and the factory's own improvement. ELU gives them what they can't get from their own computer: what real users do and where they fail, browser agents that use your app like a real user, a verdict on every pull request, and a real-user comparison on every release.

## Use it

Give a Grok Bot access to your repository and say:

```
Read https://raw.githubusercontent.com/ELU-Labs/dark-factory/main/PROMPT.md and follow it for this repository.
```

## You need

- A web app in a GitHub repository.
- Branch protection on the default branch. It's free on public repositories; private ones need a paid GitHub plan.

The factory signs you in to ELU with one link (new accounts get $1,000 of credit), and builds previews, CI, and progressive deploys if they're missing.

Optional: an API key for a model outside the Grok family, so the Verifier doesn't share the Builders' blind spots.

## Improve it

Ran it and something broke? Open an issue with what Grok did and where it stopped, or send a pull request against [PROMPT.md](PROMPT.md).
