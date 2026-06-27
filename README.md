# Cavale

**Cavale** /ka.val/ — French for *galloping* / *running free*. A training
companion for **ultra-trail running** and the strength work that supports it.

This is the **meta-repository**. It owns no application code itself — it links
the individual parts together as **git submodules**, each part living in its own
repository.

## Repository map

| Part   | Path   | Repository                                   | Stack                       |
|--------|--------|----------------------------------------------|-----------------------------|
| API    | `api/` | [`cavale-api`](https://github.com/MauriceNDS/cavale-api) | Java 25 · Spring Boot 4 · Postgres |
| Web    | `web/` | _(planned)_ `cavale-web`                     | _(frontend, TBD)_           |
| Infra  | `infra/` | _(planned)_                                | Docker Compose · deploy     |

Each part has its own README, issues, and CI. This repo pins each part at a
specific commit via the submodule pointer.

## Cloning

Clone **with submodules** (one step):

```bash
git clone --recursive git@github.com:MauriceNDS/cavale.git
```

Already cloned without `--recursive`? Initialise them after the fact:

```bash
git submodule update --init --recursive
```

## Daily workflow — the two-step commit (READ THIS)

Submodules are pointers to a commit in another repo. The golden rule:

> **Commit & push inside the part first. THEN bump the pointer in the meta repo.**

Concretely, when you change the API:

```bash
# 1. work inside the part, like a normal repo
cd api
git add -A && git commit -m "feat(training): add plan endpoint"
git push                       # push the PART first — non-negotiable

# 2. record the new pointer in the meta repo
cd ..
git add api                    # stages the updated submodule commit
git commit -m "chore: bump api"
git push                       # push the META
```

If you push the meta before pushing the part, anyone cloning gets a dangling
pointer. Part first, always.

## Keeping submodules up to date

```bash
git pull --recurse-submodules          # pull meta + checked-out submodule commits
git submodule update --remote --merge  # fast-forward submodules to their latest remote
```

## Adding a new part later

```bash
git submodule add git@github.com:MauriceNDS/cavale-web.git web
git commit -m "chore: add web submodule"
```

## Status

🚧 Early development, backend-first. The API is where the work is happening —
see [`api/`](api) and its `docs/ROADMAP.md`.
