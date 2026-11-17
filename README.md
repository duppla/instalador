# instalador

Deja listo el computador de un ingeniero de Duppla con una sola línea: git, gh, Node, uv, Python, jq, gcloud, Tailscale y Claude Code con el plugin `duppla-core`. También entra a Google, GitHub y Tailscale, configura git y npm para los paquetes `@duppla/*` y al final revisa que todo funcione. Se puede correr varias veces: solo hace lo que falta.

Este repo es público a propósito: el script se baja antes de que la persona tenga GitHub. **Aquí nunca van llaves ni datos.**

## Antes de empezar

- Correo `@duppla.co`. El script lo saca del login de Google; no va en la línea.
- Una cuenta de GitHub ya invitada a la organización `duppla`. Si la invitación está pendiente, el script la acepta.
- Ser administrador del computador.

## La línea

Mac, en la Terminal:

```
bash <(curl -fsSL https://instalar.duppla.co/mac)
```

Windows, en PowerShell normal (no como administrador):

```
irm https://instalar.duppla.co/windows | iex
```

Para solo revisar el computador sin instalar nada:

```
bash <(curl -fsSL https://instalar.duppla.co/mac) doctor
$env:DUPPLA_DOCTOR=1; irm https://instalar.duppla.co/windows | iex
```

## Cómo funciona la URL

`instalar.duppla.co` es una regla de redirección en Cloudflare hacia la rama `estable` de este repo:

| Ruta | Va a |
|---|---|
| `/mac` | `https://raw.githubusercontent.com/duppla/instalador/estable/mac.sh` |
| `/windows` | `https://raw.githubusercontent.com/duppla/instalador/estable/windows.ps1` |

Lo que hay en `main` **no** lo corre nadie: solo lo que llega a `estable`. Así un merge normal no cambia lo que instalan las personas nuevas. (No se usan GitHub Releases porque la organización no deja crear etiquetas.)

## Publicar una versión

1. Cambio por PR a `main`. El CI revisa la sintaxis de los dos scripts (bash 3.2 + shellcheck, y Windows PowerShell 5.1).
2. Probarlo en un computador de verdad, bajando el archivo de la rama del PR:
   - Mac: `bash <(curl -fsSL https://raw.githubusercontent.com/duppla/instalador/<rama>/mac.sh)`
   - Windows: `irm https://raw.githubusercontent.com/duppla/instalador/<rama>/windows.ps1 | iex`
3. Publicar: PR de `main` a `estable`, diciendo en qué computadores se probó.
   ```
   gh pr create --base estable --head main --title "Publicar: <qué cambió>"
   ```
   GitHub sirve los archivos con hasta 5 minutos de caché: el cambio llega a todos en ese tiempo.

Publicar cambia lo que corre en el computador de cada persona nueva: es un permiso sensible. `main` y `estable` están protegidas (PR con una aprobación y CI en verde, sin force push).

Si algo sale mal, se revierte el merge en `estable` con otro PR (`git revert`).
