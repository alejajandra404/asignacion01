# Taller Mecánico — Mapeo a Prisma

Proyecto de mapeo del dominio "Taller mecánico" a Prisma con MySQL, para la
Unidad II de Tópico de Aplicaciones Web.

## Modelos

- **Cliente**: nombre, teléfono y correo (único)
- **Vehiculo**: placa (única), marca, modelo y año — relacionado a un Cliente
- **OrdenServicio**: descripción, fecha de ingreso y estado (enum) — relacionada a un Vehiculo
- **Refaccion**: nombre y precio — relacionada a una OrdenServicio

## Enum

`EstadoOrden`: `abierta`, `en_proceso`, `entregada`, `cancelada`

## Cómo levantar el proyecto

1. Instalar dependencias: `npm install`
2. Configurar `.env` con tu propia cadena de conexión a MySQL:
   ```
   DATABASE_URL="mysql://root:TU_PASSWORD@localhost:3306/taller_mecanico"
   ```
3. Ejecutar la migración inicial: `npx prisma migrate dev --name init`

## Pregunta

**¿Qué pasaría si intentaras borrar un Cliente que todavía tiene un Vehículo?**

La migración define la llave foránea de `Vehiculo` hacia `Cliente` con
`ON DELETE RESTRICT`. Esto significa que MySQL no permite borrar un Cliente
mientras exista al menos un Vehículo relacionado con él — la base de datos
rechaza el `DELETE` y lanza un error de restricción de llave foránea
(foreign key constraint fails). Para poder borrar al Cliente, primero
habría que borrar o reasignar todos sus Vehículos.
