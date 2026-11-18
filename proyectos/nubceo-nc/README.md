# Proyecto Nubceo NC

Propuesta a **Nubceo** (partner) para el **Calendario de Notas de Crédito (NC)**, con acceso desde FarmaPay Comercios vía SSO.

> Este archivo es la memoria del proyecto: cualquier sesión nueva debe leerlo primero y actualizarlo al terminar.

## Insumos (`insumos/`)

| Archivo | Qué es |
|---|---|
| `flujo-calendario-nc.jpg` | Mapa de pantallas del flujo (Inicio → Calendarios → panel del día → detalle de NC) |
| `frames-video/` | Cuadros cada 4 s del video demo (48 s, sin audio, 2026-11-02). El .mp4 original (111 MB) no se versiona |
| `guia-sso-partners.pdf` / `.txt` | Guía de integración SSO para partners v1.0 (OIDC, Authorization Code + PKCE S256) |

## Lo que muestra el prototipo (datos de demostración, v1.97.0)

**Accesos al calendario**
- Menú izquierdo → *Calendarios*.
- Inicio → botón *Ver calendarios* y tarjeta de acceso rápido *Calendarios* (NUEVO: "ingresos y notas de crédito en una sola vista").

**Vista Calendarios** (pestañas *Todo / Ingresos / Notas de crédito*, filtros, búsqueda por liquidación o lote)
- KPIs: Ingresos del mes ($1.897.057,00), Notas de crédito del mes ($1.213.545,31 · 28 NC · 4 financiadores), Total consolidado ($3.110.602,31), Próximo movimiento.
- Grilla mensual: cada día muestra un bloque azul (ingresos con logos de plataformas) y uno violeta (NC con logos de financiadores). Navegación entre meses y *Generar reporte*.

**Ingresos del día — experiencia actual** (panel lateral)
- Totales por plataforma (Farmapay, Mercado Pago, Payway) y tabla de liquidaciones: ID, plataforma, bruto, deducciones, neto, estado.

**Notas de crédito del día — NUEVO** (panel lateral)
- Totales por financiador (PAMI, OSDE, SMG) y tabla: ID de lote, financiador, monto liquidación, cobertura %, monto NC, info y comprobante. Exportar y descargar.

**Detalle de nota de crédito** (navegable 1 de N)
- Datos del lote: convenio, fecha de cierre, nº de lote, período, fecha teórica de liquidación, droguería de recupero, fecha de emisión de la NC, nº de comprobante legal.
- Montos: importe PUP, % de cobertura, importe NC laboratorio, monto NC.
- Nota abierta: "Equivalencia con monto liquidación a confirmar".

## SSO: puntos clave de la guía
- Solo **OIDC** está validado: Authorization Code + PKCE S256, cliente confidencial (el canje se hace en el backend). SAML 2.0 es potencial y no está comprometido.
- Componentes: Comercios/FarmaPay (origen), BHUB (comprobante firmado), Keycloak operado por el cliente (issuer OIDC), Partner (app, usuarios, sesión, permisos).
- `sub` = id de BHUB y es la clave de identidad. El email no está verificado: **no se vincula por email**.
- El token **no** trae CUIT, comercio, tenant ni roles. No hay refresh token ni logout coordinado. Tokens de 300 s y sesión Keycloak de 10 min.
- Entregables del partner: checklist técnico (§6), matriz de esfuerzo (§7), casos T01–T13 (§8) y formulario de devolución (§9).

## Entregables
- **Documento de propuesta (Claude Docs):** https://claude.ai/artifact/LYNCVyyPzwpwgnLqsWPVjx
  Secciones: resumen, problema, solución pantalla por pantalla, reparto de roles, datos requeridos, SSO (diagrama), plan y matriz de esfuerzo, criterios de aceptación (C01–C08, T01–T13), riesgos y próximos pasos.

- **Prototipo navegable (artifact):** https://claude.ai/artifact/GMQmyjfdtXmqVcZozjKgKG
  Fuente: `prototipo/calendario-nc.html`. Replica el video y la imagen de flujo. Los links por pantalla son `#inicio`, `#calendarios`, `#calendarios-creditos`, `#creditos-2`, `#ingresos-2` y `#nc-67210334`.
  Los datos de demo cierran con el video: ingresos $1.897.057, NC $1.213.545,31 (28 NC, 4 financiadores) y consolidado $3.110.602,31.

- **Presentación de producto (Slides):** https://claude.ai/artifact/F3AN5CvcU2B1nG1JP9ow4T
  Fuente: `presentacion/project/`. Tiene 13 slides (contexto, problema, T+1, rol de FarmaPay, propuesta, experiencia, datos, SSO, visión del "frontend del tesorero", roles y próximos pasos), con notas del orador.

## Reunión 2026-11-17 (Nubceo: Santiago, Fernanda, Silvana, Luciano, Agustina)
- Transcripción en `insumos/reunion-2026-11-17-transcripcion.*`.
- Las NC las emite Farmalink (lo usa ~90% de la industria). Son crédito en la cuenta corriente de la droguería para productos de un laboratorio.
- Objetivo de la industria: adelantar la NC del laboratorio de 60–120 días a T+1, con checks antifraude (receta digital, transacción FarmaPay, software de gestión).
- La visión de Nubceo es ser el "frontend del tesorero", y las NC encajan. A futuro: pagar a droguerías con NC y financiar.
- Nubceo ya implementó SSO (Active Directory, para otro cliente). La estructura técnica "no es de cero".
- Próximos pasos: FarmaPay comparte los 2 docs de SSO y el set de datos de NC (validar con Juan). Nubceo arma su propuesta de SSO y analiza la viabilidad. La mesa de industria sería en enero (la industria cierra del 15/20 de diciembre al 5 de enero).
- Optimizador fiscal / alertas: idea para más adelante, menos prioritaria.

## Decisiones
- 2026-11-17: **Nubceo construye** el calendario; FarmaPay aporta el prototipo, las especificaciones y el SSO.
- 2026-11-17: el formato es un **documento** (no deck), para audiencia de **negocio + técnica**.

## Pendientes
- [ ] Confirmar qué fecha ubica la NC en el calendario: emisión (supuesto actual) o fecha teórica de liquidación.
- [ ] Regla de cálculo del monto NC (PUP × cobertura vs. importe NC laboratorio).
- [ ] Estados posibles de una NC.
- [ ] Mapeo usuario → farmacia → lotes en Nubceo (el token SSO no lo trae).

## Historial
- 2026-11-17: arranque del proyecto, carga de insumos.
- 2026-11-17: primera versión del documento de propuesta.
- 2026-11-17: prototipo navegable publicado y enlazado desde el documento.
- 2026-11-18: transcripción de la reunión cargada; presentación de producto publicada.
