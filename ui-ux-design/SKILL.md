---
name: ui-ux-design
description: Designs and implements UI/UX using React, Next.js, and Tailwind CSS following Vercel/v0-inspired design principles. Use when building user interfaces, designing components, creating layouts, implementing design systems, or reviewing UI/UX for web applications.
---

# UI/UX Design — Quanture Standards

Inspired by Vercel v0 design principles: clean, minimal, functional, accessible.

> ⚠️ **SIEMPRE** leer [reference/brand-identity.md](reference/brand-identity.md) antes de crear cualquier componente visual. Toda UI debe respetar la identidad de marca Quanture: colores `#002D72` (azul), `#FFD700` (amarillo), tipografía sans-serif, logo sin modificaciones.

## Stack

- **Framework**: Next.js 14+ (App Router)
- **Styling**: Tailwind CSS + shadcn/ui
- **Language**: TypeScript
- **Icons**: Lucide React

## Design principles

1. **Clarity over cleverness** — UI should be obvious without explanation
2. **Consistency** — same patterns, same spacing, same colors everywhere
3. **Accessibility** — WCAG AA minimum: contrast ratios, keyboard nav, ARIA labels
4. **Mobile first** — design for small screens, enhance for large
5. **Performance** — no layout shift, no blocking renders

## Component pattern (shadcn/ui)

```tsx
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"

interface DashboardCardProps {
  title: string
  value: string
  description?: string
}

export function DashboardCard({ title, value, description }: DashboardCardProps) {
  return (
    <Card>
      <CardHeader className="flex flex-row items-center justify-between space-y-0 pb-2">
        <CardTitle className="text-sm font-medium">{title}</CardTitle>
      </CardHeader>
      <CardContent>
        <div className="text-2xl font-bold">{value}</div>
        {description && (
          <p className="text-xs text-muted-foreground">{description}</p>
        )}
      </CardContent>
    </Card>
  )
}
```

## Spacing scale (Tailwind)

| Usage | Class |
|---|---|
| Micro spacing | `gap-1`, `p-1` (4px) |
| Component internal | `gap-2`, `p-2` (8px) |
| Card padding | `p-4` (16px) |
| Section spacing | `py-8` (32px) |
| Page sections | `py-16` (64px) |

## Color system

Use CSS variables via shadcn/ui — never hardcode hex values in components:

```tsx
// GOOD
<div className="bg-background text-foreground">
<div className="bg-muted text-muted-foreground">
<div className="bg-primary text-primary-foreground">

// AVOID
<div style={{ backgroundColor: "#1a1a1a" }}>
```

## Responsive layout

```tsx
// Mobile-first grid
<div className="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-4">
```

## Accessibility rules

- Every `<img>` has `alt`
- Interactive elements have `aria-label` when text isn't descriptive enough
- Focus states: never `outline-none` without replacement
- Color is never the only differentiator
- Form fields have visible labels (not just placeholder)

## Forms

```tsx
import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import * as z from "zod"

const schema = z.object({
  email: z.string().email("Email inválido"),
  name: z.string().min(2, "Mínimo 2 caracteres"),
})

export function ContactForm() {
  const form = useForm({ resolver: zodResolver(schema) })

  return (
    <Form {...form}>
      <FormField
        name="email"
        render={({ field }) => (
          <FormItem>
            <FormLabel>Email</FormLabel>
            <FormControl>
              <Input placeholder="tu@empresa.com" {...field} />
            </FormControl>
            <FormMessage />
          </FormItem>
        )}
      />
    </Form>
  )
}
```

## Security in UI

- Sanitize user content before rendering: use `DOMPurify` if rendering HTML
- Never use `dangerouslySetInnerHTML` with untrusted data
- Store tokens in `httpOnly` cookies, not `localStorage`
- CSRF protection on all mutation endpoints

## Advanced reference

- **Identidad de marca Quanture** (obligatorio): See [reference/brand-identity.md](reference/brand-identity.md)
- **Design tokens**: See [reference/tokens.md](reference/tokens.md)
- **Component library**: See [reference/components.md](reference/components.md)
- **Animations**: See [reference/animations.md](reference/animations.md)
