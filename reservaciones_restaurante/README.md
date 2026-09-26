# Reservaciones de Restaurante — Mapeo a Prisma

Proyecto de mapeo del dominio "Reservaciones de restaurante" a Prisma con MySQL,
para la Unidad II de Tópico de Aplicaciones Web.

## Modelos

- **Cliente**: nombre, teléfono y correo (único)
- **Mesa**: número (único) y capacidad
- **Turno**: nombre (por ejemplo comida o cena), hora de inicio y hora de fin
- **Reservacion**: fecha y estado (enum) — relacionada a un Cliente, a una Mesa y a un Turno

## Enum

`EstadoReservacion`: `confirmada`, `cancelada`, `completada`

## Restricción única

`Reservacion` marca como única la combinación de `mesaId` y `turnoId`
(`@@unique([mesaId, turnoId])`).

## Cómo levantar el proyecto

1. Instalar dependencias: `npm install`
2. Configurar `.env` con tu propia cadena de conexión a MySQL:
   ```
   DATABASE_URL="mysql://root:TU_PASSWORD@localhost:3306/reservaciones_restaurante"
   ```
3. Ejecutar la migración inicial: `npx prisma migrate dev --name init`

## Pregunta

**¿Por qué esa combinación única evita reservar la misma mesa dos veces en el mismo turno?**

Al marcar `mesaId` y `turnoId` juntos como únicos con `@@unique([mesaId, turnoId])`,
MySQL crea un índice único sobre esas dos columnas combinadas. Esto significa que
no pueden existir dos registros en `Reservacion` con la misma pareja de mesa y
turno al mismo tiempo: si ya existe una reservación para la Mesa 5 en el Turno
"cena", cualquier intento de insertar otra reservación con esa misma combinación
es rechazado por la base de datos con un error de restricción única. Así se
evita reservar la misma mesa dos veces en el mismo turno.
