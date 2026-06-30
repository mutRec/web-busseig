# web-busseig - sóc Balena

Blog personal de busseig recreatiu - https://busseig.abellot.net/

## Tecnologies

- **Hugo** - Static site generator
- **Tema**: BeautifulHugo
- **Docker** + **Nginx** per desplegament

## Contingut

- 62 articles sobre busseig recreatiu
- Punts d'immersió: Costa Brava i litoral català
- Galeria fotogràfica
- Diari de busseig

## Desenvolupament local

1. Instal·lar Hugo a Windows: https://gohugo.io/installation/windows/
2. Executar el servidor de desenvolupament:

```bash
hugo server
```

## Desplegament amb Docker

### Multi-stage build

El Dockerfile utilitza un build multi-stage:

1. **Stage 1**: Alpine Linux + Hugo per generar els fitxers HTML
2. **Stage 2**: Nginx 1.25 Alpine per servir el lloc web

### Executar

```bash
docker compose up -d
```

El lloc web quedarà disponible a: http://localhost:85

## Flux de treball

### 1. Desenvolupament a Windows

- Desenvolupar i provar localment amb Hugo
- Crear nous posts dins de `content/post/`
- Crear pàgines dins de `content/page/`
- Afegir imatges a `static/img/`

### 2. Copia de fitxers al servidor

Utilitzar WinSCP per copiar els canvis al servidor POMA:

- Posts: `content/post/`
- Pàgines: `content/page/`
- Imatges: `static/img/`
- Configuració: `hugo.toml`

### 3. Desplegament al servidor POMA

Connectar al servidor via RDM (Remote Desktop Manager) i executar:

```bash
# Parar i eliminar el contenidor actual
docker stop web-busseig
docker rm web-busseig

# Eliminar la imatge antiga
docker rmi web-busseig:0.9

# Executar el nou desplegament
docker compose up -d
```

## Estructura del projecte

```
web-busseig/
├── archetypes/        # Plantilles per a nous posts
├── content/
│   ├── page/         # Pàgines estàtiques
│   └── post/         # Articles del blog (62 posts)
├── layouts/          # Plantilles personalitzades
├── static/
│   ├── css/          # Estils personalitzats
│   ├── fontawesome/  # Icones Font Awesome
│   ├── fonts/        # Tipus de lletra
│   ├── img/          # Imatges del lloc
│   └── js/           # Scripts JavaScript
├── themes/
│   └── beautifulhugo/ # Tema utilitzat
├── docker-compose.yml
├── Dockerfile
└── hugo.toml         # Configuració de Hugo
```
