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

## Decisiones / pendientes
- [ ] Formato y audiencia de la propuesta
- [ ] Alcance: ¿qué construye Nubceo y qué aporta FarmaPay (datos de NC, API)?

## Historial
- 2026-11-17: arranque del proyecto, carga de insumos.
