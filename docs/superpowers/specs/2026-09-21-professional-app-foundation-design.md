# Base profesional de aplicaciones con estrategia *free-first*

Este cambio prepara a los agentes para diseñar aplicaciones rápidas de validar sin usar la velocidad ni los planes gratuitos como excusa para límites débiles, acceso inseguro a datos o baja calidad visual.

## Camino rápido

1. Empieza estático, local o con servicios gestionados gratuitos; incorpora código de servidor solo ante un requisito comprobado.
2. Mantén la política de dominio independiente del framework, SDK del proveedor y detalles de transporte.
3. Trata calidad visual, accesibilidad, movimiento seguro y estados responsive como criterios de aceptación.

## Decisiones

| Área | Decisión |
| --- | --- |
| Acoplamiento | Minimizar acoplamiento innecesario, no perseguir el imposible “acoplamiento cero”. Depender de abstracciones estables en límites volátiles. |
| Cohesión | Mantener cada módulo enfocado en una capacidad de negocio o responsabilidad técnica. |
| Coste | *Free-first* es obligatorio. Infraestructura pagada o autoalojada requiere una excepción escrita: requisito no cubierto, alternativas gratuitas consideradas, coste mensual/operativo y plan de salida. |
| Backend | Preferir generación estática, APIs del navegador, almacenamiento local y BaaS gestionado. Usar una función edge para una operación confiable y corta; usar backend persistente solo para procesos duraderos, integraciones privadas, orquestación compleja o restricciones que las opciones previas no cubran. |
| Seguridad de datos | El acceso del navegador a datos gestionados exige políticas de mínimo privilegio como RLS; secretos y credenciales privilegiadas nunca llegan al cliente. |
| Calidad UX | Tokens del sistema de diseño, movimiento intencional, diseño responsive y estados de carga/error/vacío son requisitos, no acabado opcional. |
| Modelo de conocimiento | Las skills son contratos de decisión compactos para agentes. Enlazan referencias locales específicas en vez de duplicar prosa enciclopédica. |
| Idioma | Las skills, instrucciones y documentación creadas en este repositorio se escriben en español. Las skills externas instaladas pueden conservar su idioma original. |

## Nuevas skills

| Skill | Responsabilidad | No reemplaza |
| --- | --- | --- |
| `software-architecture` | límites, dirección de dependencias, puertos/adaptadores y ADRs | `frontend-architecture` |
| `design-patterns` | selección de patrones GoF, empresariales, de integración y distribuidos | decisiones de arquitectura |
| `free-first-architecture` | escalera obligatoria de coste e infraestructura | guía específica de despliegue por proveedor |
| `backendless-apps` | static-first, PWA, local-first, BaaS, RLS y funciones edge | `security` y `database-design` |
| `product-discovery` | hipótesis, alcance MVP, experimentos y señales de éxito | planificación de implementación |
| `design-system` | tokens, contratos de componentes, temas y consistencia visual | `ui-design` y `accessibility` |
| `motion-design` | animación con propósito, rendimiento y movimiento reducido | flujos de interacción de producto |
| `visual-quality` | regresión visual, responsive y cobertura de estados | pruebas funcionales |
| `deployment-strategy` | hosting gratuito, CI/CD, límites, observabilidad y disparadores de migración | configuración de un proveedor |

## Escalera de decisión *free-first*

1. **Estático/local:** sitio estático, almacenamiento del navegador, datos falsos, prototipo solo local o PWA.
2. **Nivel gratuito gestionado:** frontend alojado más autenticación/base de datos/almacenamiento gestionados, con política explícita de acceso desde cliente.
3. **Edge/serverless:** código mínimo sin estado para secretos, webhooks, transformaciones o acciones privilegiadas.
4. **Backend dedicado:** solo si hay procesos persistentes, trabajos de larga duración, integraciones privadas complejas o restricciones de plataforma demostradas.
5. **Autoalojamiento:** permitido solo con un plan operativo de disponibilidad, monitorización, copias de seguridad, parches de seguridad, exposición de red y recuperación.

Cada escalamiento registra el requisito, nivel inferior descartado, escala esperada, coste, responsable operativo y ruta de salida o migración.

## Alcance del catálogo de patrones

`design-patterns` cubre los 23 patrones GoF y los patrones profesionales habituales empresariales, de integración, datos, concurrencia, resiliencia y sistemas distribuidos. No prescribirá patrones por nombre: cada entrada indicará problema, fuerzas, colaboradores, beneficios, costes, señales de mal uso y una alternativa más simple.

## Restricciones y riesgos

- Los planes gratuitos y sus reglas de uso comercial, cuota, pausa o retención cambian. La skill de despliegue exigirá verificar las condiciones vigentes antes de elegir proveedor.
- Los servicios gestionados reducen operación, pero introducen riesgo de límite con proveedor; adaptadores y rutas de exportación de datos deben ser explícitos.
- Un servidor doméstico no es “gratis”: electricidad, fiabilidad, seguridad, restricciones del ISP, copias de seguridad y tiempo operativo son costes reales.

## Verificación

- Cada nuevo `SKILL.md` usa frontmatter válido y el contrato de skills del proyecto.
- `AGENTS.md` enlaza cada skill del proyecto y hace visible la regla de excepción *free-first*.
- El registro de skills lista cada skill nueva.
- `git diff --check` pasa.

## Fuera de alcance

- Elegir framework, proveedor o librería de componentes predeterminados.
- Implementar una aplicación o aprovisionar servicios remotos.
- Reescribir skills existentes salvo conflicto específico.

## Siguiente paso

Revisar esta especificación antes de planificar la implementación de las nueve skills y las actualizaciones de `AGENTS.md`.
