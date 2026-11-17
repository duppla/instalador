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

`instalar.duppla.co` es una regla de redirección en Cloudflare hacia la última versión publicada en GitHub Releases:

| Ruta | Va a |
|---|---|
| `/mac` | `https://github.com/duppla/instalador/releases/latest/download/mac.sh` |
| `/windows` | `https://github.com/duppla/instalador/releases/latest/download/windows.ps1` |

Lo que hay en `main` **no** lo corre nadie: solo lo que se publica como release. Así un merge no cambia lo que instalan las personas nuevas.

## Publicar una versión

1. Cambio por PR. El CI revisa la sintaxis de los dos scripts.
2. Probarlo en un computador de verdad, bajando el archivo de la rama:
   - Mac: `bash <(curl -fsSL https://raw.githubusercontent.com/duppla/instalador/<rama>/mac.sh)`
   - Windows: `irm https://raw.githubusercontent.com/duppla/instalador/<rama>/windows.ps1 | iex`
3. Merge y publicar:
   ```
   gh release create vX.Y.Z mac.sh windows.ps1 --title vX.Y.Z --notes "qué cambió y en qué se probó"
   ```

Publicar cambia lo que corre en el computador de cada persona nueva: es un permiso sensible. Solo publican los administradores del repo.

Si algo sale mal, se vuelve a la versión anterior marcándola como la última: `gh release edit vX.Y.Z --latest`.
