# Research: Eliminar posts publicados con assets

Fuente principal: DeepWiki — https://deepwiki.com/Enveloppe/obsidian-enveloppe

---

## ¿Enveloppe puede gestionar la eliminación?

**Sí.** El plugin tiene un mecanismo nativo llamado `autoclean` (desde v7.2.0 incluye soporte para assets).

### Mecanismo: función `deleteFromGithub` (`src/GitHub/delete.ts`)

1. Obtiene todos los archivos en el repositorio GitHub
2. Los compara contra todos los archivos actualmente con `share: true` en Obsidian
3. Los que están en el repo pero ya no tienen `share: true` → marcados para borrado
4. Si `includeAttachments: true` → también borra imágenes/assets asociados
5. Ejecuta borrado vía GitHub API

### Trigger: comando manual "Purge"

El comando `purgeCallback` está disponible en la paleta de comandos de Obsidian.
No es automático al guardar — se ejecuta a demanda.

---

## Configuración actual del proyecto

En `.agents/config/enveloppe.json`:

```json
"autoclean": {
  "includeAttachments": true,
  "enable": false,       ← DESHABILITADO
  "excluded": []
}
```

`includeAttachments` ya está en `true`, solo falta habilitar `enable`.

---

## Opciones disponibles

### Opción A — Habilitar autoclean (automatizado)

Cambiar en la config de Enveloppe (Obsidian settings):
- `autoclean.enable: true`
- `autoclean.includeAttachments: true` (ya está)
- `autoclean.excluded: []` → agregar paths a excluir si es necesario (soporta regex)

**Comportamiento**: al ejecutar "Purge", borra posts sin `share: true` + sus assets.

**Riesgo**: si `excluded` no está bien configurado, puede borrar archivos que no deberían borrarse.

### Opción B — Eliminación manual (situación actual)

1. En Obsidian: cambiar `share: false` en frontmatter del post
2. Ejecutar comando "Purge depublished and deleted files" desde paleta
3. Si assets quedan huérfanos: borrarlos manualmente de `static/assets/` en el repo

**Ventaja**: control total, sin riesgo de borrados accidentales.

### Opción C — Override por nota con frontmatter

Se puede controlar `autoclean` por nota individual:
```yaml
autoclean: false   # evita que esta nota específica sea eliminada por autoclean
```

---

## ¿Se suele hacer con script?

No es la práctica habitual en este ecosistema. Las técnicas más comunes son:

1. **Usar el comando Purge de Enveloppe** (recomendado, nativo)
2. **GitHub Action de limpieza** — script bash que compara `content/posts/` contra `static/assets/` y borra assets sin post asociado. Útil cuando autoclean no cubre todos los casos.
3. **Borrado manual directo en el repo** — editar el repo desde GitHub web o git local.

---

## Modo `dryRun`

Enveloppe tiene un modo `dryRun` que simula la eliminación sin tocar el repo real.
Útil para verificar qué se borraría antes de ejecutar el Purge real.

---

## Recomendación para este proyecto

**Corto plazo (sin cambiar config):**
1. Cambiar `share: false` en el post a eliminar
2. Ejecutar "Purge" desde Obsidian
3. Verificar que `includeAttachments: true` borre los assets (ya está configurado)
4. Hacer commit/push manual si el workflow no lo dispara solo

**Si se quiere habilitar autoclean permanentemente:**
- Habilitar `enable: true` en Enveloppe settings
- Probar primero con `dryRun` para ver qué borraría
- Agregar exclusiones necesarias en `excluded`
