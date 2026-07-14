# 🦊 Exiled Exchange 2 — StarFox fork

Fork de [Kvan7/Exiled-Exchange-2](https://github.com/Kvan7/Exiled-Exchange-2) (base v0.15.8),
que a su vez es fork de [Awakened PoE Trade](https://github.com/SnosMe/awakened-poe-trade).
Todo el crédito del overlay original a sus autores; este fork agrega widgets y mejoras propias.

> **English summary at the bottom.** ⬇️

## ✨ Qué agrega este fork

### 🕐 Historial de price checks (widget nuevo)
- Cada price check queda registrado con el rango de precio visto en los primeros resultados.
- Panel con filtro de búsqueda; click en un ítem lo re-checkea al instante.
- Botón para mandar cualquier ítem del historial a la lista de compras.

### 🛒 Lista de compras (widget nuevo)
- Carpetas renombrables (Tabletas / Armas y armaduras / Anillos / Otros + personalizadas)
  con auto-categorización de ítems.
- Guarda el **último precio visto** de cada ítem y hace cuánto se vio.
- **Búsquedas guardadas (variantes)**: desde el price check se puede guardar la combinación
  exacta de filtros/mods de un ítem; al re-checkearla se re-aplican los filtros y el precio
  de la variante se actualiza solo.
- Drawers laterales de historial y compras integrados a la ventana del price check,
  con pin persistente.

### 💰 Analizador de stash (widget nuevo)
- Lee una pestaña del stash vía API oficial (requiere POESESSID) y la valúa completa:
  divisas/únicos/gemas al instante con poe.ninja, rares uno por uno vía trade API
  (respetando rate limits, con presupuesto configurable).
- Síntesis de texto de ítem desde el JSON de la API para reusar el parser completo del overlay.

### ⚖️ Comparador de ítems + ayuda de craft (widget nuevo)
- Hotkey sobre un ítem en el suelo/stash: panel con stats comparadas (DPS, defensas, mods)
  contra tu equipo actual (vía API oficial de personajes — *en desarrollo, ver Known issues*).
- Pestaña Craft: prefijos/sufijos abiertos, tiers de cada mod y sugerencias básicas.
- Botones directos a poe2db, Craft of Exile y price check.

### 🔧 Mejoras al price check
- Botón **guardar búsqueda** (con precio) y botón **copiar link de trade** (URL bien
  encodeada — los links ya no se cortan en ligas con espacios).
- Tooltips de resultados: comparación ▴/▾ de cada stat contra tu ítem (matcheo por hash).
- Stats del ítem listadas debajo de los resultados.

### 🌐 Otros
- Textos nuevos traducidos a **9 idiomas** (en/es completos + de/fr/pt/ru/ja/ko/zh-Hant).
- Soporte preliminar del parser para ítems **Sanctified** (formato aún no confirmado).
- **Auto-update deshabilitado a propósito** para que no pise el asar moddeado
  (la app avisa que hay versión nueva pero no la descarga).
- 21 tests nuevos para los stores (suite completa: 522 en verde), ESLint y Prettier limpios.
- 🥚 Easter egg: `Ctrl + Alt + Shift + M` en el juego…

## ⚠️ Avisos
- El analizador de stash y el comparador usan tu **POESESSID** (cookie de sesión de
  pathofexile.com). Se guarda **en texto plano** en `%APPDATA%\exiled-exchange-2\apt-data\config.json`
  y solo se envía a pathofexile.com — no compartas tu `config.json` con nadie.
- La valuación de rares requiere el cliente/app en **inglés**.
- GGG no tiene API pública documentada para todo esto; los endpoints pueden cambiar.

## 🔨 Build e instalación sobre la app oficial

```bash
cd renderer
npm install
npm run make-index-files
npx vue-tsc --noEmit && npx vitest run   # typecheck + tests
npx vite build

cd ../main
npm install --ignore-scripts
npm run build
```

Para instalar sobre una instalación oficial de EE2 (`resources/app.asar`):
1. Extraer el `app.asar` original: `npx @electron/asar extract app.asar app-src`
2. Copiarle encima el contenido de `resources/app.asar.unpacked/` (módulos nativos).
3. Reemplazar `main.js`, `vision.js`, `assets/`, `data/`, `images/`, `index.html` con los
   artefactos de build.
4. Re-empaquetar:
   `npx @electron/asar pack app-src app.asar --unpack-dir "{node_modules/electron-overlay-window,node_modules/uiohook-napi}"`
5. Reemplazar `resources/app.asar` (haciendo backup del original primero).

## 🐛 Known issues
- La comparación contra el equipo del personaje (`/api/character/poe2`) todavía no anda
  de forma confiable; el panel muestra diagnóstico de lo que pudo leer.
- El formato de ítems Sanctified en el portapapeles no está confirmado.

## Licencia
Misma licencia que el proyecto original (ver [LICENSE](LICENSE)). Este fork no está
afiliado a GGG ni a los autores del proyecto original.

---

# 🇬🇧 English summary

Fork of [Kvan7/Exiled-Exchange-2](https://github.com/Kvan7/Exiled-Exchange-2) (v0.15.8 base)
adding four new overlay widgets and QoL features:

- **Price-check history**: every check is logged with the observed price range; click to
  re-check, one click to pin items to the shopping list.
- **Daily shopping list**: renamable folders with auto-categorization, last-seen price per
  item, and **saved searches (variants)** that re-apply your exact stat filters on re-check.
  History/shopping side-drawers built into the price-check window.
- **Stash value analyzer**: values a whole stash tab (poe.ninja for currency/uniques/gems,
  trade API one-by-one for rares, rate-limit aware). Requires POESESSID.
- **Item compare + craft helper**: compare a hovered item against your equipped gear
  (official character API — WIP), open prefix/suffix count, mod tiers and quick suggestions.
- Price-check extras: save-search button, **copy trade link** (properly URL-encoded),
  per-stat ▴/▾ comparison in result tooltips.
- New UI strings translated to 9 languages; preliminary Sanctified item parsing;
  auto-update intentionally disabled (so it never overwrites the modded asar);
  21 new store tests (522 total, green).


**Warning**: the stash analyzer / gear compare use your POESESSID, stored in plain text in
your local `config.json` and sent only to pathofexile.com. Don't share your config.
Rare-item valuation requires the English client.
