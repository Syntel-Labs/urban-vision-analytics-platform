# 01 – Contexto

## 1. Introducción

Este proyecto consiste en una plataforma de analítica visual en tiempo real orientada a la detección y análisis de personas y vehículos a partir de fuentes de video (RTSP, webcam o archivos).

El sistema no se limita a la detección de objetos; su objetivo principal es generar métricas accionables derivadas del flujo y comportamiento observado en el video.

- La detección es el medio técnico.
- La analítica es el producto.

---

## 2. Problema

En entornos urbanos y comerciales existen cámaras disponibles que generan grandes volúmenes de información visual, pero esa información no se transforma automáticamente en métricas útiles.

Algunos problemas comunes:

- No existe conteo automático confiable.
- No se mide densidad o congestión en tiempo real.
- No se generan estadísticas históricas comparables.
- Los sistemas existentes suelen ser costosos o cerrados.
- Muchas soluciones se enfocan solo en detección, no en analítica.

Existe una oportunidad de construir una plataforma que:

- Procese video en tiempo real.
- Detecte y rastree entidades.
- Genere métricas agregadas.
- Visualice resultados de forma clara y profesional.

---

## 3. Propuesta

Desarrollar una plataforma de analítica de flujo urbano y movilidad que permita:

- Detectar personas y vehículos.
- Contar entradas y salidas.
- Calcular densidad y congestión.
- Generar métricas por minuto.
- Visualizar resultados en un dashboard en tiempo real.
- Persistir estadísticas históricas.

El sistema estará diseñado para funcionar tanto en CPU como en GPU, manteniendo la misma arquitectura y cambiando únicamente el entorno de ejecución.

---

## 4. Enfoque

El proyecto adopta un enfoque de investigación + implementación + documentación:

- Investigación: selección y evaluación de modelos, librerías y patrones arquitectónicos.
- Implementación: construcción modular siguiendo principios de separación de responsabilidades.
- Documentación: decisiones técnicas justificadas y trazables mediante ADRs.

El objetivo no es solo construir una demo funcional, sino demostrar capacidad de diseño de sistemas de visión computacional en tiempo real con arquitectura limpia y escalable.

---

## 5. Alcance conceptual del producto

El sistema se posiciona como una plataforma de:

- Analítica de tráfico y flujo peatonal.
- Monitoreo de congestión.
- Generación de métricas operativas en tiempo real.

No se incluye reconocimiento facial ni identificación individual.
El enfoque es estadístico y agregado, no biométrico.

---

## 6. Audiencia

Este proyecto está orientado a:

- Evaluadores técnicos.
- Reclutadores.
- Ingenieros interesados en arquitectura de sistemas de visión.
- Equipos que busquen referencia de implementación modular en Python con streaming en tiempo real.

---

## 7. Objetivos del proyecto

1. Demostrar diseño arquitectónico sólido.
2. Integrar visión computacional con backend y frontend en tiempo real.
3. Manejar procesamiento distribuido mediante mensajería.
4. Exponer métricas en un dashboard profesional.
5. Documentar decisiones técnicas de forma estructurada.

---

## 8. No objetivos

- Reconocimiento facial.
- Identificación de personas específicas.
- Implementación de Kubernetes en esta versión.
- Uso de microservicios adicionales innecesarios.
- Optimización extrema para producción masiva.
