# Component Patterns Reference

## Data table

```tsx
import {
  Table, TableBody, TableCell,
  TableHead, TableHeader, TableRow
} from "@/components/ui/table"

export function DataTable({ data }: { data: Item[] }) {
  return (
    <div className="rounded-md border">
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead>Nombre</TableHead>
            <TableHead className="text-right">Valor</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          {data.map((item) => (
            <TableRow key={item.id}>
              <TableCell className="font-medium">{item.name}</TableCell>
              <TableCell className="text-right">{item.value}</TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </div>
  )
}
```

## Loading skeleton

```tsx
import { Skeleton } from "@/components/ui/skeleton"

export function CardSkeleton() {
  return (
    <Card>
      <CardHeader>
        <Skeleton className="h-4 w-[250px]" />
      </CardHeader>
      <CardContent>
        <Skeleton className="h-8 w-[100px]" />
      </CardContent>
    </Card>
  )
}
```

## Empty state

```tsx
export function EmptyState({ title, description, action }: EmptyStateProps) {
  return (
    <div className="flex flex-col items-center justify-center rounded-lg border border-dashed p-8 text-center">
      <h3 className="text-lg font-semibold">{title}</h3>
      <p className="mt-2 text-sm text-muted-foreground">{description}</p>
      {action && <div className="mt-4">{action}</div>}
    </div>
  )
}
```

## Toast notifications

```tsx
import { toast } from "sonner"

// Success
toast.success("Guardado correctamente")

// Error
toast.error("Error al guardar. Intenta de nuevo.")

// Info
toast.info("Procesando...")
```
