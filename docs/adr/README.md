# Architectural Decision Records (ADR)

## 1. Propósito

Este directorio contiene los registros formales de decisiones arquitectónicas relevantes del proyecto.

Un ADR documenta:

- El problema.
- El contexto.
- La decisión tomada.
- Sus consecuencias.

---

## 2. ¿Cuándo crear un ADR?

Se debe crear un ADR cuando:

- Se elige una tecnología clave.
- Se define un patrón arquitectónico.
- Se descarta una alternativa significativa.
- Se modifica una decisión estructural importante.

No se crean ADR para detalles menores de implementación.

---

## 3. Convención de nombres

Formato:

```bash
0001-titulo-de-la-decision.md
0002-otra-decision.md
```

Reglas:

- Numeración incremental.
- Un archivo por decisión.
- No reutilizar números.
- No borrar ADRs históricos; si una decisión cambia, crear uno nuevo que lo reemplace.

---

## 4. Flujo recomendado

1. Crear archivo usando el template.
2. Marcar estado como “Propuesto”.
3. Revisar decisión.
4. Cambiar estado a “Aceptado”.
5. Referenciar el ADR desde la documentación principal si es necesario.

---

## 5. Relación con otros documentos

- 04-tech-stack.md debe referenciar ADRs tecnológicos.
- 05-architecture-overview.md puede referenciar ADRs estructurales.
- 10-deployment.md puede referenciar ADRs de infraestructura.
