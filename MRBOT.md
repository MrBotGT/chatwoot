# Chatwoot para Mr. Bot Salud

Fork de [chatwoot/chatwoot](https://github.com/chatwoot/chatwoot) para el soporte de Mr. Bot Salud
(`soporte.mrbot.cloud`, centro de ayuda en `ayuda.mrbot.cloud`). Es la **edición comunitaria** con nuestra marca.

## Licencia

Chatwoot es MIT, salvo la carpeta `enterprise/`, que tiene su propia licencia (ver `LICENSE`). Esta rama **no tiene
`enterprise/`** (igual que la imagen `-ce` que publica Chatwoot), así que todo lo que queda es MIT. El aviso de copyright
de `LICENSE` se mantiene.

## Qué cambia (rama `mrbot`, desde el tag `v4.18.0`)

| Archivo | Cambio |
|---|---|
| `enterprise/`, `spec/enterprise/` | Fuera (edición comunitaria) |
| `docker/Dockerfile` | `ENV CW_EDITION="ce"` al final |
| `.github/workflows/` | Fuera los de Chatwoot; `mrbot-imagen.yml` publica la imagen en GHCR |
| `config/installation_config.yml` | `INSTALLATION_NAME` y `BRAND_NAME` «Mr. Bot Salud»; `BRAND_URL` y `WIDGET_BRAND_URL` a salud.mrbot.cloud |
| `public/brand-assets/logo*.svg`, `public/favicon.ico` | Logos y favicon de Mr. Bot |
| `app/views/public/api/v1/portals/_footer.html.erb` | Pie del centro de ayuda con nuestro logo y nombre |
| `app/views/public/api/v1/portals/documentation_layout/_footer.html.erb` | Sin «Hecho con» |
| `app/javascript/shared/assets/fonts/Manrope/` | Manrope (OFL), incluida en la imagen |
| `app/javascript/portal/application.scss` | El centro de ayuda usa Manrope |
| `app/javascript/shared/components/Branding.vue` | El chat no muestra «Desarrollado por …» |
| `app/javascript/widget/composables/useAvailability.js`, `Availability/AvailabilityText.vue` (+ spec) | Con un bot conectado, el chat está «en línea» y «Responde al instante» |
| `app/javascript/widget/components/AgentMessage.vue` | Distintivo «Respondió la IA» en los mensajes del bot |
| `app/javascript/widget/i18n/locale/es.json`, `en.json` | Textos `MRBOT.AI_REPLIED` y `REPLY_TIME.INSTANTLY` |
| `app/views/public/api/v1/portals/_hero.html.erb` | Cabecera azul como la portada del centro de ayuda (arte de «Marketing y soporte») |
| `app/views/public/api/v1/portals/_header.html.erb` | Logo de Mr. Bot (claro y oscuro) si el portal no tiene uno propio |
| `app/controllers/public/api/v1/portals/base_controller.rb` | Carga `LOGO` y `LOGO_DARK` para ese logo |
| `app/helpers/portal_helper.rb` | Un ícono `/brand-assets/ayuda/<nombre>.svg` se muestra como imagen |
| `public/brand-assets/ayuda/` | Íconos de las 8 categorías y la grilla de la portada |
| `MRBOT.md` | Este archivo |

Los valores de `installation_config.yml` solo se siembran en una base nueva. En el servidor de soporte los deja en la
base `deploy/soporte/desplegar.sh` del repo de la app (con un `rails runner`).

## Imagen

Cada tag `v<versión de Chatwoot>-mrbot.<n>` construye `docker/Dockerfile` (amd64) y publica
`ghcr.io/mrbotgt/chatwoot:<tag>` (paquete público). El digest queda en el resumen del workflow; el servidor de soporte
fija la imagen por ese digest en `deploy/soporte/docker-compose.yml`.

## Actualizar a una versión nueva de Chatwoot

```sh
git fetch https://github.com/chatwoot/chatwoot.git tag v<nueva> --no-tags
git checkout mrbot && git merge v<nueva>          # o: git rebase --onto v<nueva> v4.18.0 mrbot
git rm -r --ignore-unmatch enterprise spec/enterprise
# resolver solo los archivos de la tabla de arriba; los workflows nuevos de Chatwoot se vuelven a sacar
git tag v<nueva>-mrbot.1 && git push origin mrbot v<nueva>-mrbot.1
```

Antes de subir de versión, leer las notas de Chatwoot (migraciones, variables nuevas) y respaldar la base del soporte.
