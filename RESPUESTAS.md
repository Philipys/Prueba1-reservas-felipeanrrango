## Escenarios que elegí y por qué

Incluí solapamiento parcial y una reserva contenida para cubrir dos formas de choque
(`specs/001-reservas-sala/spec.md:24`, `specs/001-reservas-sala/spec.md:27`). También cubrí menos de
10 minutos, exactamente 10 minutos y otra sala (`specs/001-reservas-sala/spec.md:30`,
`specs/001-reservas-sala/spec.md:33`, `specs/001-reservas-sala/spec.md:36`). Las pruebas ejercitan esos
casos (`test/crear_reserva_test.dart:43`, `test/crear_reserva_test.dart:83`,
`test/crear_reserva_test.dart:103`, `test/crear_reserva_test.dart:142`).

## Riesgo más grave del repositorio

La consulta de reservas y el guardado son operaciones separadas (`lib/domain/crear_reserva.dart:16`,
`lib/domain/crear_reserva.dart:28`), así que dos solicitudes simultáneas podrían superar la consulta.
La base solo valida que el fin sea posterior al inicio (`supabase/migracion.sql:11`); no impone el
margen de 10 minutos.

## ¿La regla protege la app real?

La pantalla ahora envía la solicitud a `CrearReserva` (`lib/presentation/reserva_page.dart:61`), que
consulta y valida las reservas de esa sala (`lib/domain/crear_reserva.dart:16`,
`lib/domain/crear_reserva.dart:18`). Esto protege las solicitudes sucesivas; la base aún no impide
choques si dos usuarios reservan simultáneamente (`supabase/migracion.sql:11`).
