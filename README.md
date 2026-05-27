# 🤖 Asistente Virtual Inteligente — Mayorista Zeus

Sistema de atención automatizada desarrollado con **n8n + IA + Telegram**, diseñado para optimizar consultas comerciales, validación de clientes y acceso seguro a información de stock.

---

# 🚀 Descripción del Proyecto

Este proyecto implementa un **asistente virtual inteligente** para **Mayorista Zeus**, orientado a automatizar la atención al cliente mediante conversaciones naturales, rápidas y seguras.

El asistente funciona a través de **Telegram** y utiliza un **AI Agent** integrado en **n8n**, capaz de:

✅ Mantener memoria conversacional
✅ Validar clientes automáticamente
✅ Consultar stock de productos
✅ Respetar reglas comerciales definidas
✅ Responder utilizando voseo argentino
✅ Limitar accesos según permisos y contexto

El sistema fue pensado para ofrecer una experiencia similar a la atención humana, pero escalable y disponible 24/7.

---

# 🧠 Tecnologías Utilizadas

| Tecnología           | Función                                   |
| -------------------- | ----------------------------------------- |
| n8n                  | Orquestación de flujos y automatizaciones |
| Telegram Bot API     | Canal de comunicación con clientes        |
| AI Agent / LLM       | Motor conversacional inteligente          |
| Window Buffer Memory | Memoria persistente por usuario           |
| Base de Datos / API  | Consulta de stock y datos comerciales     |

---

# 🏗 Arquitectura General

## 📸 Flujo Principal del Agente

![Flujo Principal](Flujo_Agente.jpg)

El flujo principal del sistema conecta Telegram con un AI Agent desarrollado en n8n, incorporando:

* Validación automática de clientes
* Memoria persistente
* Consulta segura de stock
* Herramientas conectadas al agente
* Salidas diferenciadas para clientes y no clientes
* Integración con Vector Store para contexto adicional

### Componentes del Flujo

| Nodo                | Función                          |
| ------------------- | -------------------------------- |
| Telegram Trigger    | Recepción de mensajes            |
| IF                  | Validación inicial del usuario   |
| AI Agent            | Motor conversacional inteligente |
| Cohere Chat Model   | Modelo LLM utilizado             |
| Simple Memory       | Persistencia conversacional      |
| consultar_stock     | Consulta de inventario           |
| Simple Vector Store | Recuperación contextual (RAG)    |

---

## 🧩 Flujo RAG (Retrieval-Augmented Generation)

![Flujo RAG](Flujo_RAG.jpg)

El proyecto también incorpora un flujo de preparación documental mediante embeddings y almacenamiento vectorial.

Este módulo permite que el agente pueda:

* Consultar documentación empresarial
* Recuperar contexto relevante
* Responder preguntas basadas en información cargada
* Escalar hacia sistemas RAG más avanzados

### Proceso del Flujo RAG

```text
Documentos → Loader → Embeddings → Vector Store → AI Agent
```

### Componentes Utilizados

| Nodo                | Función                  |
| ------------------- | ------------------------ |
| HTTP Request        | Obtención de documentos  |
| Default Data Loader | Procesamiento documental |
| Embeddings Cohere   | Generación de embeddings |
| Simple Vector Store | Almacenamiento vectorial |

---

El sistema se divide en varios módulos principales:

## 1️⃣ Entrada de Mensajes

### Telegram Trigger

Recibe mensajes en tiempo real desde Telegram y activa automáticamente el flujo.

### Validación Inicial

El sistema identifica al usuario mediante `chat.id` y determina:

* Si es cliente validado
* Si tiene permisos para consultar stock
* Si debe recibir únicamente información general

---

## 2️⃣ Núcleo Inteligente (AI Agent)

El agente IA funciona como cerebro conversacional del sistema.

### Funcionalidades:

* Comprensión contextual
* Memoria conversacional
* Razonamiento lógico
* Aplicación de reglas comerciales
* Respuestas personalizadas

### Memoria Persistente

Se utiliza `Window Buffer Memory` asociada al:

```js
{{ $json.message.chat.id }}
```

Esto permite que el asistente recuerde:

* Estado de validación
* Contexto de conversación
* Consultas previas
* Flujo actual del usuario

---

## 3️⃣ Herramientas Integradas

### 🔧 consultar_stock

Herramienta protegida que permite acceder a información de inventario.

El acceso solo se habilita si el AI Agent confirma que el usuario es cliente.

Esto evita:

* Exposición innecesaria de datos
* Consultas públicas de stock
* Uso indebido de la herramienta

---

# 📋 Prompt del Sistema

El comportamiento del asistente se controla mediante un **System Prompt** robusto.

## Objetivos del Prompt

* Mantener coherencia en respuestas
* Aplicar reglas comerciales
* Definir personalidad del agente
* Controlar permisos y restricciones
* Garantizar seguridad conversacional

### Ejemplo:


> “Eres el asistente virtual de Mayorista Zeus, ubicado en Quilmes. Utilizas voseo argentino y un tono cercano y profesional.
>
> Antes de consultar stock, verificás si el usuario es cliente. Si no está validado, brindás únicamente información general e invitás al registro.
>
> Recordás el estado del usuario durante toda la conversación.
>
> Horarios:
> Lunes a Viernes: 7:45 a 16:00
> Sábados: 7:45 a 12:30
>
> Medios de pago:
> Efectivo, Transferencia, Mercado Pago y Cuenta DNI.”

---

# 🔄 Flujo General del Sistema

```text
Usuario → Telegram → n8n → AI Agent → Validación → Herramientas → Respuesta
```

---

# ⚙️ Configuración del Proyecto

## 1. Instalar n8n

Se puede utilizar:

* n8n Cloud
* Docker
* Instalación local
* Railway

---

## 2. Configurar Telegram Bot

Crear un bot mediante BotFather y agregar el token en:

```text
Telegram Trigger → Credentials
```

---

## 3. Configurar Memoria

Agregar nodo:

```text
Window Buffer Memory
```

Session ID:

```js
{{ $json.message.chat.id }}
```

---

## 4. Conectar Base de Datos

La herramienta `consultar_stock` puede conectarse a:

* APIs internas
* MySQL
* PostgreSQL
* Google Sheets
* Airtable
* ERP externos

---

## 5. Activar el Flujo

Ejecutar el workflow en modo:

```text
Production
```

para mantener el asistente online 24/7.

---

# 🛡 Seguridad Implementada

Uno de los objetivos principales del proyecto fue incorporar medidas básicas de seguridad para proteger la información comercial.

## Medidas Aplicadas

### ✅ Validación por chat.id

El usuario no necesita ingresar credenciales manualmente.

### ✅ Restricción de herramientas

La IA no puede consultar stock libremente.

### ✅ Prompt Engineering

Se limitan comportamientos no deseados mediante reglas explícitas.

### ✅ Segmentación de accesos

Clientes y no clientes reciben distintos niveles de información.

### ✅ Manejo de errores

Se recomienda agregar nodos de fallback para:

* Errores de API
* Fallos de conexión
* Consultas inválidas

---

# 📈 Posibles Mejoras Futuras

* Botones interactivos en Telegram
* Integración con WhatsApp
* Dashboard administrativo
* Sistema de autenticación avanzado
* Logs y monitoreo
* RAG con documentación empresarial
* Integración con CRM
* Análisis de conversaciones
* Escalabilidad multiusuario

---

# 💡 Objetivo del Proyecto

Este desarrollo busca demostrar cómo la combinación de:

* Automatización
* Inteligencia Artificial
* Flujos conversacionales
* Integración de herramientas

puede mejorar significativamente la atención al cliente y reducir tiempos operativos en entornos comerciales reales.

---

# 📌 Estado del Proyecto

🟢 En desarrollo activo

Actualmente se continúan realizando mejoras relacionadas con:

* Seguridad
* Escalabilidad
* UX conversacional
* Integraciones externas
* Optimización del agente IA

---

# 🤝 Contribuciones

Las contribuciones, sugerencias e ideas son bienvenidas.

Posibles áreas para colaborar:

* Optimización de prompts
* Seguridad de agentes IA
* Integraciones con APIs
* Mejoras UX
* Automatizaciones avanzadas

---

# 👩‍💻 Autor

Proyecto desarrollado como práctica de automatización e integración de IA aplicada a procesos comerciales reales.

---

# ⭐ Demo Conceptual

```text
Cliente: “¿Tenés stock de yerba Playadito?”

IA:
“¡Hola! 😊 Antes de consultar stock necesito validar si sos cliente registrado de Mayorista Zeus.”
```

---

# 📄 Licencia

Uso educativo y demostrativo.

