# web-busseig - sóc Balena

Blog personal de busseig recreatiu - https://busseig.abellot.net/

## Tecnologies

- **Hugo** - Generador de llocs estàtics (versió >= 0.146.0)
- **Tema**: [PaperMod](https://github.com/adityatelange/hugo-PaperMod) (copiat directament al directori `themes/PaperMod/`)
- **Docker** + **Nginx** per al desplegament
- Desenvolupament local amb **Docker Compose** a Windows

## Contingut

- 69 articles sobre busseig recreatiu viscut pel litoral català
- Punts d'immersió: Costa Brava i litoral català
- Galeria fotogràfica
- Accés al Diari de busseig (app sócBalena)
- Pàgina de contacte i enllaços

## Desenvolupament local

### Amb Docker (recomanat)

```bash
docker compose up -d
```

El lloc web quedarà disponible a: http://localhost:85

### Amb Hugo instal·lat localment

Si teniu Hugo instal·lat al vostre ordinador:

```bash
hugo server -D
```

El servidor de desenvolupament arrencarà a http://localhost:1313.

## Crear nous posts

Els posts es troben a `content/post/` en format Markdown.

### Usar l'arquetip (recomanat)

```bash
hugo new content post/nom-del-post.md
```

Això crea un fitxer amb el front matter predefinit:

```markdown
+++
date = '2026-08-19T12:00:00+02:00'
draft = true
title = 'Nom Del Post'
+++
```

### Estructura típica d'un post

```markdown
+++
title = 'Títol del post'
subtitle = 'Subtítol opcional'
date = '2026-08-19'
+++

Contingut del post en Markdown...

![Descripció de la imatge](/img/nom-imatge.jpg)
```

### Paràmetres disponibles al front matter

| Paràmetre   | Descripció                              | Opcional |
|-------------|-----------------------------------------|----------|
| `title`     | Títol del post                          | No       |
| `subtitle`  | Subtítol que apareix sota el títol      | Sí       |
| `date`      | Data de publicació (YYYY-MM-DD)         | No       |
| `draft`     | `true` per amagar el post fins publicar | Sí       |
| `tags`      | Etiquetes per categoritzar              | Sí       |
| `image`     | Imatge de portada del post              | Sí       |

### Imatges

Les imatges dels posts van a la carpeta `static/img/`. Al Markdown es referencien així:

```markdown
![Descripció](/img/ruta/fitxer.jpg)
```

## Pàgines estàtiques

Les pàgines principals es troben a `content/page/`:

| Pàgina            | Fitxer                        | URL                      |
|-------------------|-------------------------------|--------------------------|
| Tots els posts    | `content/page/totselsposts.md` | `/page/totselsposts/`    |
| Punts d'immersió | `content/page/puntsimmersio.md` | `/page/puntsimmersio/`  |
| Galeria fotos     | `content/page/galeriafotos.md` | `/page/galeriafotos/`   |
| Enllaços          | `content/page/links.md`       | `/page/links/`           |
| Contacte          | `content/page/contacte.md`    | `/page/contacte/`        |

## Desplegament

El flux de treball és: **desenvolupament local → push a GitHub → pull al servidor → reconstruir Docker**.

### 1. Desenvolupament local (Windows)

1. Fer els canvis necessaris (posts, pàgines, imatges, configuració...)
2. Provar localment amb `docker compose up -d` o `hugo server -D`
3. Verificar que tot funciona correctament

### 2. Push a GitHub

```bash
git add .
git commit - missatge-descripciu
git push origin main
```

### 3. Desplegament al servidor Linux

Connectar al servidor via SSH i executar:

```bash
# Navegar al directori del projecte
cd /ruta/al/projecte/web-busseig

# Aturar i eliminar el contenidor actual
docker compose down

# Pull dels nous canvis des de GitHub
git pull origin main

# Reconstruir i iniciar el contenidor
docker compose up -d --build
```

**Important**: `docker compose down` atura i elimina el contenidor. `docker compose up -d --build` reconstrueix la imatge Docker (Hugo genera els fitxers HTML de nou) i arrenca el nou contenidor amb Nginx.

## Dockerfile - Multi-stage build

El Dockerfile utilitza un build en dos etapes:

1. **Stage 1** (`alpine:latest`): Instal·la Hugo i genera els fitxers HTML estàtics
2. **Stage 2** (`nginx:1.25-alpine`): Serveix el lloc web amb Nginx al port 80

## Estructura del projecte

```
web-busseig/
├── archetypes/
│   └── default.md          # Plantilla per a nous posts
├── assets/
│   └── css/extended/
│       └── menu-hover.css  # Estils hover del menú
├── content/
│   ├── page/               # 5 pàgines estàtiques
│   └── post/               # 69 articles del blog
├── layouts/
│   ├── page/
│   │   └── totselsposts.html
│   └── shortcodes/
│       ├── figure.html
│       └── gallery.html
├── static/
│   ├── css/                # Estils personalitzats (main.css, fonts, etc.)
│   ├── fontawesome/        # Icones Font Awesome
│   ├── fonts/              # Tipus de lletra
│   ├── img/                # Imatges del lloc
│   └── js/                 # Scripts JavaScript
├── themes/
│   └── PaperMod/           # Tema PaperMod (copiat al repo)
├── docker-compose.yml      # Configuració Docker
├── Dockerfile              # Build multi-stage (Hugo + Nginx)
└── hugo.toml               # Configuració de Hugo
```

## Configuració important (hugo.toml)

- **Base URL**: `https://busseig.abellot.net/`
- **Idioma**: Català (`ca`)
- **Tema**: PaperMod
- **Menú**: 6 opcions (Articles, Punts d'immersió, Galeria, Diari Busseig, Enllaços, Contacte)
