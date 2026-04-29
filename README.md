
# 📦 FiadoApp

FiadoApp es una aplicación Android nativa que digitaliza el sistema de crédito informal entre almaceneros y sus clientes de barrio. Reemplaza el cuaderno de fiados con una solución en tiempo real, con historial de precios dinámicos, pagos por Mercado Pago y notificaciones push.

---

## 🧠 Concepto central

El almacenero **nunca pierde**. Al momento del cobro, el sistema aplica automáticamente:

precio_a_cobrar = MAX(precio_histórico, precio_actual)

Si el precio subió, se cobra el nuevo. Si bajó, se cobra el precio del día del fiado. El saldo del cliente se recalcula en tiempo real cada vez que el almacenero actualiza un precio.

---

## 👥 Roles

| Rol | Descripción |
|-----|-------------|
| **Almacenero** | Registra fiados, gestiona el catálogo con historial de precios, cobra y genera reportes PDF |
| **Cliente** | Ve su deuda estimada en tiempo real, consulta el historial y paga por Mercado Pago |

---

## ✨ Funcionalidades principales

- Registro de fiados por ticket con múltiples productos (almacenado como ítems individuales internamente)
- Catálogo de productos con historial de precios y alertas de desactualización
- Cálculo de saldo dinámico con regla `MAX(precio_histórico, precio_actual)`
- Distribución de pagos **FIFO** (más antiguo primero), cancelando ítems completos
- Sobrante de pago guardado como **saldo a favor** aplicado automáticamente al próximo pago
- Pago por **Mercado Pago** con confirmación vía webhook en Cloud Functions
- Registro de pagos en efectivo
- Límite de crédito por cliente con alerta al 80% y bloqueo automático al 100%
- Vinculación almacenero-cliente por **código de 6 dígitos**
- Notificaciones push (FCM) para fiados, pagos y alertas de límite
- Generación de **resumen mensual en PDF** (iText)
- Catálogo base precargado con ~40 productos típicos del almacén argentino

---

## 🛠️ Stack tecnológico

| Tecnología | Uso |
|-----------|-----|
| Java | Lenguaje de desarrollo Android |
| MVVM + LiveData/Flow | Patrón arquitectónico |
| Coroutines | Operaciones asíncronas |
| Firebase Auth + Google Sign-In | Autenticación |
| Firestore | Base de datos en tiempo real |
| Cloud Functions | Lógica de negocio en servidor |
| Firebase Cloud Messaging | Notificaciones push |
| Mercado Pago SDK | Procesamiento de pagos |
| iText | Generación de PDF local |

---

## 🗄️ Modelo de datos (Firestore)

```
users            → perfil de almaceneros y clientes
almacenes        → datos del negocio + código de vinculación
cuentas          → relación cliente-almacén (saldo_total, saldo_a_favor, estado)
items            → cada producto fiado (producto_id, cantidad, precio_histórico, fecha)
pagos            → pagos confirmados con detalle FIFO
productos        → catálogo de productos del almacén
precios          → historial de precios con fecha_desde / fecha_hasta
notificaciones   → historial de notificaciones por usuario
```

---

## 📐 Arquitectura

Android App (Kotlin/MVVM)
- Firebase Auth          → autenticación
- Firestore              → datos en tiempo real
- Cloud Functions        → lógica crítica de negocio
  - actualizar saldo al cargar ítem
  - regla MAX(precio_histórico, precio_actual)
  - distribución FIFO de pagos
  - webhook de Mercado Pago
  - alertas de límite de crédito
- FCM                    → notificaciones push
- Mercado Pago SDK       → pagos integrados

---

## 📋 Documentación

- 📄 Requerimientos Funcionales y No Funcionales [Google docs](https://docs.google.com/document/d/1WNPsiSW3ItQMDzGpQALtifBF6bhwRk4OHGIVOfgGkJE/edit?usp=sharing)
- 📱 Diseño de pantallas: [Figma](https://www.figma.com/design/iDAZfrOnG1b95HmLzdEOJa/FiadosApp?node-id=0-1&t=0QuIR3chSyMIebhK-1)

---

## 🔍 Apps similares analizadas

Ninguna de las siguientes resuelve el fiado con precios dinámicos ni la regla `MAX(histórico, actual)`:

1. [Fiado App – Matancita](https://play.google.com/store/apps/details?id=com.matancita.appuntalo) — montos fijos, sin catálogo de productos
2. [Crediado](https://play.google.com/store/apps/details?id=tv.digicash.efiado) — fiados sin historial de precios ni stock de productos
3. [Deudores – Registro de cobros](https://play.google.com/store/apps/details?id=com.brunox.deudores) —  app de deudas personales, sin almacén
4. [Credito facil](https://play.google.com/store/apps/details?id=com.fiadofacil) — sin integración de pagos


---

## 🎓 Contexto académico

Proyecto final de **Laboratorio VI (PUI)** — Universidad Nacional de Santiago del Estero (UNSE)  
Año: 2026

---

## 📸 Pantallas

