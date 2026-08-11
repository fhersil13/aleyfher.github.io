# Despliegue

## Cómo funciona hoy

`.github/workflows/deploy.yml` corre en cada push a `master` (o manual
con "Run workflow"): sube todo el repo como artefacto de Pages y lo
despliega con las actions oficiales de GitHub —
`actions/checkout@v7`, `actions/configure-pages@v6`,
`actions/upload-pages-artifact@v5`, `actions/deploy-pages@v5`. No hay
build: se sube el repo tal cual, porque no hay paso de compilación.

Requisito en GitHub: **Settings → Pages → Source = "GitHub Actions"**
en el repo `valentine` (no "Deploy from a branch" — eso usaría el
workflow automático de GitHub en vez de este).

URL actual: `https://fherneysilva.github.io/valentine/`.

## Historial: por qué la URL cambió varias veces

Vale la pena dejarlo por escrito porque cada síntoma parecía un bug
distinto y en realidad eran tres cambios de cuenta/repo encadenados.

1. **Rename de usuario de GitHub** (`fhersil13` → `fherneysilva`): el
   deploy empezó a fallar con `Invalid audience` en el token OIDC — el
   workflow automático de Pages tenía cacheado el username viejo. Se
   resolvió creando un workflow propio (este archivo) en vez de
   depender del workflow interno de GitHub, lo que fuerza a regenerar
   el token con el owner actual.
2. **Rename del repo** (`aleyfher.github.io` → `valentine`): cambia el
   path de la URL. Project pages se sirven en
   `https://<usuario>.github.io/<nombre-del-repo>/` — el segmento del
   path es literalmente el nombre del repo, sea cual sea.
3. **El repo de la cuenta se dejó de llamar `fherneysilva.github.io`**
   (se renombró a `fherneysilva` para poder usarlo como README del
   perfil de GitHub). Esto es importante porque **GitHub solo trata un
   repo como "user site" si se llama exactamente
   `<usuario>.github.io`**. Efecto en cadena:
   - Deja de servir la raíz del dominio custom.
   - Deja de propagar el dominio custom a los *project pages* de la
     cuenta — por eso `valentine` dejó de responder en
     `fherneysilva.com/valentine/` y volvió a su URL nativa de
     `github.io`.
   - Esto fue una decisión consciente (se prefirió tener el README de
     perfil), no un bug. Si en algún momento se quiere recuperar el
     dominio custom para este proyecto, hay que renombrar ese otro repo
     de vuelta a `fherneysilva.github.io`, o configurar un dominio
     custom propio *solo* para `valentine` (Settings → Pages → Custom
     domain de este repo, con su propio registro DNS).

En resumen: **este repo (`valentine`) no depende de ningún otro repo
para funcionar** — solo necesita su propio `Source = GitHub Actions`.
La única variable externa es si otro repo de la cuenta tiene un dominio
custom que quiera "prestarle" su URL, y hoy no lo hace.
