# Plan de implementación: base profesional *free-first*

> **Para agentes de implementación:** SUB-SKILL OBLIGATORIA: usar `superpowers:subagent-driven-development` (recomendada) o `superpowers:executing-plans` para implementar este plan tarea por tarea. Los pasos usan casillas (`- [ ]`) como seguimiento.

**Objetivo:** Añadir nueve skills en español y reglas de `AGENTS.md` para crear aplicaciones desacopladas, visualmente cuidadas y validadas sin coste por defecto.

**Arquitectura:** Las skills se agrupan por decisión: núcleo de arquitectura, validación e infraestructura, y calidad visual. Cada `SKILL.md` será un contrato compacto con frontmatter, disparadores, reglas, tabla de decisión, pasos y contrato de salida; `AGENTS.md` queda como índice y política global, no como una enciclopedia duplicada.

**Tecnologías:** Markdown, PowerShell para validación estructural, Git y `gentle-ai skill-registry` si está disponible.

**Especificación:** `docs/superpowers/specs/2026-09-21-professional-app-foundation-design.md`

## Restricciones globales

- Las skills, instrucciones y documentación creadas por el proyecto se escriben en español.
- *Free-first* es obligatorio; toda excepción de coste, backend dedicado o autoalojamiento debe quedar documentada.
- Mantener mínimo acoplamiento necesario, dependencias hacia abstracciones y máxima cohesión por responsabilidad.
- No modificar las skills existentes salvo conflicto demostrado.
- Las skills externas instaladas conservan su idioma y contenido originales.

## Foco de revisión

- Una skill recomienda un backend dedicado sin agotar y documentar las opciones de menor coste.
- Un cliente recibe secretos, credenciales privilegiadas o acceso a datos sin una política explícita de mínimo privilegio.
- Un patrón se prescribe por moda, sin problema, coste, señales de mal uso ni alternativa simple.
- Una animación ignora `prefers-reduced-motion`, rendimiento o propósito de interacción.
- Una entrada de `AGENTS.md` apunta a una skill inexistente o deja una skill nueva sin descubrir.

---

### Task 1: Crear skills del núcleo arquitectónico

**Archivos:**
- Crear: `skills/software-architecture/SKILL.md`
- Crear: `skills/design-patterns/SKILL.md`
- Crear: `skills/free-first-architecture/SKILL.md`

**Interfaces:**
- Consume: la política y la escalera de decisión de la especificación.
- Produce: tres contratos de decisión que las demás skills podrán referenciar por nombre.

- [ ] **Paso 1: Ejecutar comprobación roja de ausencia**

```powershell
@('software-architecture','design-patterns','free-first-architecture') | ForEach-Object {
  if (Test-Path "skills/$_/SKILL.md") { throw "La skill $_ ya existe" }
}
```

Resultado esperado: PASS solo si ninguna skill existe; una colisión detiene el trabajo para revisar el alcance.

- [ ] **Paso 2: Crear `software-architecture`**

Incluir frontmatter válido y las secciones `Activation Contract`, `Hard Rules`, `Decision Gates`, `Execution Steps`, `Output Contract` y `References`. Definir dirección de dependencias, límites por capacidad, puertos/adaptadores, composición raíz, anti-corrupción y ADRs mínimos. Exigir una alternativa más simple antes de introducir una capa.

- [ ] **Paso 3: Crear `design-patterns`**

Cubrir GoF, patrones empresariales, integración, datos, concurrencia, resiliencia y distribuidos con una matriz: problema, fuerzas, solución, coste, señales de mal uso, cuándo no usar y alternativa simple. Prohibir añadir patrones sin un problema concreto.

- [ ] **Paso 4: Crear `free-first-architecture`**

Implementar la escalera estático/local → BaaS gratuito → edge/serverless → backend dedicado → autoalojamiento. Exigir una excepción escrita con requisito, alternativas descartadas, coste, operador y salida/migración antes de superar un nivel.

- [ ] **Paso 5: Ejecutar comprobación verde estructural**

```powershell
$required = @('Activation Contract','Hard Rules','Decision Gates','Execution Steps','Output Contract')
Get-ChildItem skills/software-architecture,skills/design-patterns,skills/free-first-architecture -Filter SKILL.md | ForEach-Object {
  $text = Get-Content $_.FullName -Raw
  if ($text -notmatch '(?m)^name:' -or $text -notmatch '(?m)^description:') { throw "Frontmatter incompleto: $($_.FullName)" }
  foreach ($heading in $required) { if ($text -notmatch [regex]::Escape($heading)) { throw "Falta $heading en $($_.FullName)" } }
}
```

Resultado esperado: PASS.

- [ ] **Paso 6: Confirmar el foco de revisión**

Comprobar que `free-first-architecture` exige excepción antes de backend dedicado; que `design-patterns` incluye alternativa simple; y que `software-architecture` no permite que dominio dependa de proveedores.

- [ ] **Paso 7: Commit**

```bash
git add skills/software-architecture/SKILL.md skills/design-patterns/SKILL.md skills/free-first-architecture/SKILL.md
git commit -m "feat(skills): add architecture decision guides"
```

### Task 2: Crear skills de validación e infraestructura

**Archivos:**
- Crear: `skills/backendless-apps/SKILL.md`
- Crear: `skills/product-discovery/SKILL.md`
- Crear: `skills/deployment-strategy/SKILL.md`

**Interfaces:**
- Consume: `free-first-architecture`, `security`, `database-design`, `performance`.
- Produce: una ruta segura para validar una hipótesis y desplegar sin coste por defecto.

- [ ] **Paso 1: Ejecutar comprobación roja de ausencia**

```powershell
@('backendless-apps','product-discovery','deployment-strategy') | ForEach-Object {
  if (Test-Path "skills/$_/SKILL.md") { throw "La skill $_ ya existe" }
}
```

- [ ] **Paso 2: Crear `backendless-apps`**

Incluir árbol de decisión para sitio estático, PWA, almacenamiento local, BaaS y función edge. Exigir RLS y no exponer secretos o claves privilegiadas. Diferenciar una operación corta sin estado de un proceso persistente.

- [ ] **Paso 3: Crear `product-discovery`**

Exigir problema, usuario, hipótesis falsable, experimento de menor coste, métrica de éxito, umbral de decisión y fecha de revisión. Prohibir construir funcionalidades no necesarias para aprender.

- [ ] **Paso 4: Crear `deployment-strategy`**

Exigir verificar condiciones actuales, límites, uso comercial, cuotas, pausa, retención y exportación de datos del proveedor antes de seleccionarlo. Incluir observabilidad mínima, copias de seguridad y disparadores explícitos de migración.

- [ ] **Paso 5: Ejecutar comprobación verde estructural y de seguridad**

```powershell
$paths = 'skills/backendless-apps/SKILL.md','skills/product-discovery/SKILL.md','skills/deployment-strategy/SKILL.md'
if ((Get-Content $paths[0] -Raw) -notmatch 'RLS|Row Level Security') { throw 'backendless-apps debe exigir RLS' }
if ((Get-Content $paths[1] -Raw) -notmatch 'hipótesis') { throw 'product-discovery debe exigir hipótesis' }
if ((Get-Content $paths[2] -Raw) -notmatch 'cuota|límites') { throw 'deployment-strategy debe exigir límites' }
```

Resultado esperado: PASS.

- [ ] **Paso 6: Commit**

```bash
git add skills/backendless-apps/SKILL.md skills/product-discovery/SKILL.md skills/deployment-strategy/SKILL.md
git commit -m "feat(skills): add free-first validation guides"
```

### Task 3: Crear skills de calidad visual

**Archivos:**
- Crear: `skills/design-system/SKILL.md`
- Crear: `skills/motion-design/SKILL.md`
- Crear: `skills/visual-quality/SKILL.md`

**Interfaces:**
- Consume: `ui-design`, `ux-design`, `accessibility`, `frontend-architecture`, `performance`.
- Produce: criterios consistentes para sistemas visuales, movimiento y revisión de UI.

- [ ] **Paso 1: Ejecutar comprobación roja de ausencia**

```powershell
@('design-system','motion-design','visual-quality') | ForEach-Object {
  if (Test-Path "skills/$_/SKILL.md") { throw "La skill $_ ya existe" }
}
```

- [ ] **Paso 2: Crear `design-system`**

Definir tokens semánticos, contratos de componentes, variantes limitadas, composición, temas, documentación de estados y reglas para evitar valores visuales ad-hoc.

- [ ] **Paso 3: Crear `motion-design`**

Exigir objetivo comunicativo por animación, duración y curva coherentes, presupuesto de rendimiento, alternativa sin movimiento y soporte de `prefers-reduced-motion`. Prohibir animaciones decorativas que oculten latencia o bloqueen interacción.

- [ ] **Paso 4: Crear `visual-quality`**

Exigir revisión de jerarquía, contraste, responsive, foco, carga, error, vacío, éxito, contenido largo, zoom y regresión visual. Separar defectos visuales de defectos funcionales.

- [ ] **Paso 5: Ejecutar comprobación verde de calidad visual**

```powershell
if ((Get-Content 'skills/design-system/SKILL.md' -Raw) -notmatch 'token') { throw 'Faltan tokens' }
if ((Get-Content 'skills/motion-design/SKILL.md' -Raw) -notmatch 'prefers-reduced-motion') { throw 'Falta movimiento reducido' }
if ((Get-Content 'skills/visual-quality/SKILL.md' -Raw) -notmatch 'carga|error|vacío') { throw 'Faltan estados visuales' }
```

Resultado esperado: PASS.

- [ ] **Paso 6: Commit**

```bash
git add skills/design-system/SKILL.md skills/motion-design/SKILL.md skills/visual-quality/SKILL.md
git commit -m "feat(skills): add visual quality guides"
```

### Task 4: Registrar la política y las skills

**Archivos:**
- Modificar: `AGENTS.md`
- Modificar: `odd/tasks/professional-app-foundation.md`
- Modificar: `.atl/skill-registry.md` mediante el comando de actualización, si está disponible.

**Interfaces:**
- Consume: las nueve rutas `skills/<nombre>/SKILL.md` creadas en las tareas 1–3.
- Produce: políticas globales descubribles e índice completo de skills.

- [ ] **Paso 1: Comprobar que las nueve rutas existen**

```powershell
$skills = 'software-architecture','design-patterns','free-first-architecture','backendless-apps','product-discovery','deployment-strategy','design-system','motion-design','visual-quality'
$skills | ForEach-Object { if (-not (Test-Path "skills/$_/SKILL.md")) { throw "Falta $_" } }
```

- [ ] **Paso 2: Actualizar `AGENTS.md`**

Añadir una regla global *free-first* con formato de excepción obligatoria. Añadir las nueve filas al índice, con descripciones en español y enlaces relativos correctos. Explicar mínimo acoplamiento necesario y máxima cohesión sin prometer acoplamiento cero.

- [ ] **Paso 3: Actualizar el seguimiento**

Marcar FDN-02 a FDN-05 solo cuando sus archivos y comprobaciones estén observados; registrar hashes de los commits y comprobaciones reales. Mantener tareas pendientes de forma honesta.

- [ ] **Paso 4: Actualizar el registro y verificar descubrimiento**

```powershell
gentle-ai skill-registry refresh --force
$registry = Get-Content '.atl/skill-registry.md' -Raw
$skills | ForEach-Object { if ($registry -notmatch [regex]::Escape("`$_`")) { throw "No descubierta: $_" } }
```

Resultado esperado: PASS. Si el comando no está disponible, documentar la limitación y no afirmar que el registro se actualizó.

- [ ] **Paso 5: Ejecutar verificación final**

```bash
git diff --check
git status --short
```

Resultado esperado: sin errores de espacio y sin archivos ajenos preparados para commit.

- [ ] **Paso 6: Commit**

```bash
git add AGENTS.md odd/tasks/professional-app-foundation.md .atl/skill-registry.md .atl/.skill-registry.cache.json
git commit -m "docs: register free-first foundation skills"
```

## Revisión del plan

- Cobertura de especificación: tareas 1–3 crean las nueve skills; tarea 4 registra la política, enlaces y descubrimiento.
- Sin marcadores: el plan no contiene marcadores pendientes, trabajo diferido ni pasos sin resultados verificables.
- Consistencia: todas las skills siguen la misma estructura y la política *free-first* se declara tanto en la especificación como en `AGENTS.md`.
- Foco de revisión: las comprobaciones de las tareas 1–4 cubren backend prematuro, datos sin RLS, patrones sin alternativa, movimiento reducido y descubrimiento de skills.

## Entrega

El plan debe revisarse antes de implementar. La ejecución recomendada es **nativa**, porque las nueve skills comparten el mismo contrato editorial y el entorno actual no permite delegar automáticamente; se verificará cada unidad de trabajo y se realizará una revisión final del conjunto.
