# Grandvalira — Tarjeta digital de monitores de esquí

Web estática mobile-first para monitores (instructores) de esquí de Grandvalira. Cada monitor tiene su propia URL con tarjeta de presentación y formulario de valoración de clases.

Demo: https://guillesrl.github.io/grandvalira/?id=alvaro

## Características

- Tarjeta de presentación con foto, datos de contacto y redes sociales
- Sección "Sobre mí" con botones de propina (Wise + Bizum)
- Badge con la puntuación media del monitor en tiempo real
- Encuesta de satisfacción de 9 preguntas (escala 1–5 estrellas) + 2 campos de texto
- Envío al webhook de n8n → almacenamiento en Neon PostgreSQL
- Diseño dark mode, glassmorphism, safe-area para móviles con notch
- Sin dependencias ni build — un único `index.html`

## Estructura

```
grandvalira/
├── index.html          # App completa (HTML + CSS + JS inline)
├── monitores.json      # Datos de cada monitor indexados por id
├── README.md
└── assets/
    ├── montanas.jpg    # Foto real de Grandvalira (banner)
    ├── ana.jpg         # Foto de Ana Martín
    ├── alvaro.jpg      # Foto de Álvaro Mena
    └── monitor-demo.svg
```

## Uso

Cada monitor tiene su URL con el parámetro `?id=`:

```
https://guillesrl.github.io/grandvalira/?id=alvaro
https://guillesrl.github.io/grandvalira/?id=ana
https://guillesrl.github.io/grandvalira/?id=demo
```

Para añadir un monitor nuevo, añadir una entrada en `monitores.json`:

```json
{
  "nuevo": {
    "nombre": "Nombre Apellido",
    "titulo": "Monitor de esquí · Nivel III",
    "foto": "assets/nuevo.jpg",
    "telefono": "+376 000 000",
    "email": "nuevo@grandvalira.com",
    "idiomas": "Español, English",
    "experiencia": "X años en Grandvalira",
    "sobre": "Texto opcional de presentación",
    "wise": "https://wise.com/pay/me/usuario",
    "whatsapp": "https://wa.me/376000000",
    "tiktok": "https://www.tiktok.com/@usuario",
    "instagram": "https://www.instagram.com/usuario"
  }
}
```

## Backend (n8n + Neon)

- Workflow n8n: `8lC7eHPWfgCFPaTg` en https://n8n.guillers.es
- Webhook POST: `https://n8n.guillers.es/webhook/grandvalira`
- Webhook GET (stats): `https://n8n.guillers.es/webhook/grandvalira-stats?id=<monitorId>`
- Base de datos: Neon PostgreSQL — proyecto `twilight-king-97709009`, base `neondb`

### Tablas

**valoraciones** — una fila por encuesta enviada:
`id, monitor_id, monitor_nombre, calidad_general, dinamismo, correccion_tecnica, progresion, trato, adaptacion, seguridad, volveria_instructor, recomendacion, lo_mejor, mejorar, media_puntaje, created_at`

**monitores** — una fila por monitor con su media acumulada:
`monitor_id, monitor_nombre, total_valoraciones, media_general, updated_at`

## Monitores activos

| id | Nombre | Nivel |
|----|--------|-------|
| demo | Carlos Vidal | Monitor · Nivel III |
| ana | Ana Martín | Monitora · Nivel III |
| alvaro | Álvaro Mena | Técnico Deportivo Grado Superior · Nivel III |
