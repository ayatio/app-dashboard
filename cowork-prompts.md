# Cowork prompts, App Dashboard

Vier prompts om het dashboard te vullen en draaiend te houden. Kopieer, plak in een Cowork‑sessie, klaar. De data staat in `apps.json`, dat is de enige bron die je moet aanpassen.

---

## 1. Scan lokale + VPS projecten en vul het dashboard

> Run dit in een Cowork‑sessie **op mijn Mac** (of in de cloud met toegang tot `~/repos`).

```
Doel: al mijn lokale projecten en VPS-services inventariseren en apps.json van het app-dashboard bijwerken.

1. Vraag toegang tot ~/repos (en losse projecten ~/train-schedule-app, ~/whatsapp-integration).
2. Loop elke submap van ~/repos. Bepaal per project:
   - naam = mapnaam
   - git remote:  git -C <dir> remote get-url origin
   - laatste commit:  git -C <dir> log -1 --format=%cd --date=short
   - tech uit de bestanden:
     * package.json deps -> react, nextdotjs, vue, svelte, vite, tailwindcss, typescript, @supabase (supabase)
     * Cargo.toml -> rust,  go.mod -> go,  requirements.txt of pyproject.toml -> python,  Package.swift -> swift
     * wrangler.toml -> cloudflare,  vercel.json of .vercel -> vercel,  netlify.toml -> netlify
     * Dockerfile of docker-compose.yml -> docker (draait op VPS),  firebase.json -> firebase
   - status: 'live' als er een deploy-config + recente commit is, anders 'wip'
3. VPS: lees ~/repos/VPS (docker-compose*.yml, Caddyfile, nginx conf) en ~/.cloudflared/*.yml.
   Elke service of hostname = een app met infra ['vps'] en de tunnel-URL als url.
4. Werk apps.json bij:
   - behoud bestaande entries met dezelfde id + hun handmatige velden
   - voeg nieuwe toe met reviewNeeded:true en zinvolle defaults
   - vul frontend/backend/infra en lastCommit in
   - sla scratch over: untitled folder, python1, python2, prep, unsubscribe-test, mousmover
5. Toon me eerst een korte diff (toegevoegd / gewijzigd) voor je opslaat.
```

---

## 2. Zet het dashboard live op GitHub Pages

> Nadat je een lege publieke repo `ayatio/app-dashboard` hebt aangemaakt op github.com/new.

```
Ik heb een lege publieke repo ayatio/app-dashboard aangemaakt.
Push de inhoud van het app-dashboard pakket (index.html, apps.json, README.md,
.nojekyll, .github/workflows/pages.yml) naar de main branch via de GitHub-tools.
De workflow zet GitHub Pages automatisch aan. Bevestig de live URL
https://ayatio.github.io/app-dashboard/ en meld het als Pages nog handmatig
aangezet moet worden (Settings > Pages > Source: GitHub Actions).
```

---

## 3. Dagelijkse auto‑sync (geplande taak)

> Gebruik dit als prompt van een **geplande taak** (dagelijks). Elke run is een verse sessie, dus de instructie staat op zichzelf.

```
Werk mijn app-dashboard bij.
1. Lees apps.json uit de repo ayatio/app-dashboard (branch main).
2. Scan mijn GitHub-repos (user:ayatio) en mijn Supabase-projecten.
3. Voeg apps toe die nog niet in apps.json staan (match op 'id') met reviewNeeded:true
   en zinvolle defaults. Behoud alle bestaande handmatige velden. Update 'generatedAt'
   en waar mogelijk de live-status.
4. Alleen als er iets wijzigde: commit de nieuwe apps.json naar main
   (Pages deployt vanzelf). Rapporteer kort wat er veranderde.
```

---

## 4. Eén app toevoegen of verrijken

```
In apps.json van ayatio/app-dashboard, zet <app-id> op:
status=<live|staging|wip|tool|idea|archived>, progress=<0-100>,
frontend=[...], backend=[...], infra=[...], url=<...>, reviewNeeded=false.
Commit naar main.
```

---

## Veld‑spiek

| veld | betekenis |
|------|-----------|
| `status` | `live` · `staging` · `wip` · `tool` · `idea` · `archived` |
| `progress` | 0–100, de voortgangsbalk |
| `frontend` / `backend` / `infra` | lijst van [simple-icons](https://simpleicons.org) slugs, bv. `react`, `supabase`, `vercel`, `cloudflarepages`, `vps`, `local` |
| `url` | waar de app draait (leeg = "nog geen URL") |
| `reviewNeeded` | `true` toont het `controleer`‑label |
