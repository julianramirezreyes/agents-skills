# Skills para una base profesional de aplicaciones

## Objetivo
Crear una base reutilizable para agentes que permita validar aplicaciones con rapidez y estrategia *free-first*, conservando límites arquitectónicos sólidos, mantenibilidad, escalabilidad y UI/UX de calidad.

## Política
- *Free-first* es obligatorio. Un servicio pagado, backend propio o servicio autoalojado requiere una excepción documentada que explique por qué las alternativas estáticas, locales, gestionadas gratuitas o edge no cubren el requisito.
- Preferir el mínimo acoplamiento necesario y la máxima cohesión; el acoplamiento cero no es un objetivo técnico significativo.
- Las skills, instrucciones y documentación creadas por el proyecto se escriben en español; las skills externas instaladas pueden conservar el idioma original.

## Tareas
- [x] FDN-01 — Escribir y revisar la especificación de arquitectura. Ruta: inline; evidencia: diseño conversacional aprobado; especificación revisada en español y confirmada en `c328f9b`; plan creado en `dbd4716`.
- [x] FDN-02 — Añadir skills de arquitectura, catálogo de patrones y decisión *free-first*. Ruta: ejecución nativa autorizada; evidencia: commit `9185035`; comprobación estructural PASS.
- [x] FDN-03 — Añadir skills de aplicaciones sin backend, descubrimiento de producto y despliegue. Ruta: ejecución nativa autorizada; evidencia: commit `05aa76b`; comprobación PASS.
- [x] FDN-04 — Añadir skills de sistema de diseño, motion design y calidad visual. Ruta: ejecución nativa autorizada; evidencia: commit `2630c1e`; comprobación PASS.
- [x] FDN-05 — Actualizar el índice y reglas globales de `AGENTS.md`. Ruta: ejecución nativa autorizada; evidencia: política e índice actualizados.
- [x] FDN-06 — Actualizar el registro de skills y verificar que todas sean descubribles. Ruta: ejecución nativa autorizada; evidencia: `gentle-ai skill-registry refresh --force` y comprobación PASS; el registro versionado fue actualizado junto con la caché generada.

## Criterios de aceptación
- Cada skill nueva sigue el contrato del repositorio y tiene disparador, proceso de decisión y contrato de salida claros.
- `AGENTS.md` enlaza todas las skills y define la regla de excepción *free-first*.
- El registro de skills descubre todas las skills nuevas.
- Las skills existentes se conservan intactas.

## Comprobaciones
- Volver a leer cada `SKILL.md` generado y el registro de `AGENTS.md`.
- Ejecutar `gentle-ai skill-registry refresh --force` si está disponible.
- Verificar con `git diff --check`.

## Progreso
- Estado actual: implementación completada; pendiente de revisión final.
- Siguiente paso: revisión final de los cambios y decisión de entrega.
