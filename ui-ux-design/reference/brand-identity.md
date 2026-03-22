# Identidad de Marca — Quanture Technologies

Fuente: Manual de Identidad Visual Quanture Technologies

## Paleta de colores (obligatoria)

| Elemento | Color | HEX | Uso |
|---|---|---|---|
| Barras / Acento principal | Amarillo | `#FFD700` | Energía, énfasis, CTAs secundarios |
| Texto / Primario | Azul oscuro | `#002D72` | Headings, texto principal, botones primarios |
| Fondo | Blanco | `#FFFFFF` | Fondo base, claridad |

### Colores de soporte (tema corporativo)

| HEX | Uso sugerido |
|---|---|
| `#4472C4` | Azul secundario, links, íconos |
| `#FFC000` | Amarillo alternativo, badges, alertas |
| `#A5A5A5` | Texto secundario, bordes sutiles |
| `#E7E6E6` | Fondos de sección, separadores |

### En Tailwind CSS

```tsx
// tailwind.config.js — agregar al theme
colors: {
  brand: {
    yellow:    "#FFD700",
    blue:      "#002D72",
    "blue-mid":"#4472C4",
    "yellow-alt": "#FFC000",
    gray:      "#A5A5A5",
    "gray-light": "#E7E6E6",
  }
}

// Uso en componentes
<h1 className="text-brand-blue font-bold">QUANTURE</h1>
<span className="text-brand-yellow">●</span>
<Button className="bg-brand-blue text-white hover:bg-brand-blue/90">
```

## Tipografía

- **Familia**: Sans-serif moderna (equivalente digital: `Inter`, `Geist`, o `Roboto`)
- **Estilo**: Letras mayúsculas para el nombre de marca, peso bold/semibold
- **Tono**: Innovador, confiable, profesional

```tsx
// next.config / layout.tsx
import { Inter } from "next/font/google"
const inter = Inter({ subsets: ["latin"] })
```

## Logo

- **Símbolo**: Lupa estilizada con barras de gráfico en amarillo (#FFD700) y mango con diseño de circuito
- **Texto**: "QUANTURE" en mayúsculas, azul oscuro (#002D72), sans-serif
- **Zona de protección**: espacio libre = ancho de la letra "Q"
- **Tamaño mínimo**: 100px digital / 2cm impresión

### Versiones

| Versión | Uso |
|---|---|
| Principal (color) | Sitio web, papelería, presentaciones |
| Monocromática | Impresiones B&W |
| Ícono (solo símbolo) | Favicon, app icons, redes sociales |

### Formatos disponibles

- `PNG` — fondo transparente (uso digital)
- `SVG` — vector (impresión alta calidad)
- `JPG` — uso general
- `PDF` — documentos corporativos

## Usos correctos ✅

- Logo sobre fondo blanco o neutro
- Orientación horizontal (símbolo + texto)
- Monocromo cuando el fondo lo requiere
- En encabezados de presentaciones, tarjetas, redes sociales con fondo claro

## Usos prohibidos ❌

- No rotar ni voltear el logo
- No cambiar colores ni tipografía
- No colocar sobre fondos con bajo contraste
- No aplicar sombras, degradados ni contornos

## Tono de comunicación

| Dimensión | Descripción |
|---|---|
| Profesional | Preciso, claro, confiable |
| Innovador | Curioso, visionario, tecnológico |
| Cercano | Accesible, humano, colaborativo |

Ejemplo de voz: *"En QUANTURE, transformamos datos en decisiones inteligentes. Nuestra tecnología está diseñada para potenciar tu negocio con soluciones ágiles y seguras."*

## Aplicación en componentes UI

```tsx
// Header corporativo
<header className="bg-white border-b border-brand-gray-light">
  <div className="flex items-center gap-2">
    <img src="/logo.png" alt="Quanture Technologies" className="h-8" />
  </div>
</header>

// Botón primario (marca)
<Button className="bg-brand-blue text-white hover:bg-brand-blue/90 font-semibold">
  Solicitar Demo
</Button>

// Acento amarillo
<div className="border-l-4 border-brand-yellow pl-4">
  <p className="text-brand-blue font-medium">Dato destacado</p>
</div>

// Badge / etiqueta
<span className="bg-brand-yellow/20 text-brand-blue text-xs font-semibold px-2 py-1 rounded-full">
  Nuevo
</span>
```
