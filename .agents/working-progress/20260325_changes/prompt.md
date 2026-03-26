# Sesión: 20260325_changes

## Idea / Objetivo
Investigar cómo eliminar un post publicado del blog junto con sus assets (imágenes u otros archivos vinculados).
- ¿Se puede gestionar desde Enveloppe (Obsidian)?
- ¿Requiere eliminación manual? ¿Cuáles son las técnicas habituales?
- ¿Se suele generar un script para esto?

## Contexto
Blog Hugo con theme Risotto, publicación via Enveloppe (Obsidian plugin).
- Posts en: `content/posts/`
- Assets en: `static/assets/`
- GitHub Actions hace fix de paths + pagefind + deploy

## Referencias
- Plugin Enveloppe config: `.agents/config/enveloppe.json`
- Workflow deploy: `.github/workflows/deploy.yml`

## Restricciones
- No modificar archivos en `themes/risotto/`
