# Feature Specification: Reservas de sala

**Feature Branch**: `001-reservas-sala`
**Created**: 2026-10-01
**Status**: Draft

## User Scenarios & Testing

### User Story 1 — Reservar una sala (Priority: P1)

Como estudiante autenticado, quiero reservar una sala de estudio por un intervalo de tiempo
para tener dónde trabajar con mi grupo.

**Why this priority**: sin reservas, la app no tiene propósito.

**Independent Test**: se puede probar creando una reserva y comprobando que queda registrada.

**Acceptance Scenarios**:

1. **Dado** que la sala A está libre, **Cuando** la reservo de 09:00 a 10:00, **Entonces** la
   reserva queda registrada a mi nombre.
2. **Dado** que elijo como inicio las 10:00 y como fin las 09:00, **Cuando** intento reservar,
   **Entonces** la reserva se rechaza con el mensaje "La hora de fin debe ser posterior a la de inicio".
3. **Dado** que otra persona tiene reservada la sala A de 09:00 a 10:00, **Cuando** intento
   reservar la sala A de 09:30 a 10:30, **Entonces** la solicitud se rechaza con el mensaje
   "La sala ya está reservada en ese horario" y no se crea una reserva.
4. **Dado** que otra persona tiene reservada la sala A de 09:00 a 10:00, **Cuando** intento
   reservar la sala A de 09:15 a 09:45, **Entonces** la solicitud se rechaza con el mensaje
   "La sala ya está reservada en ese horario" y no se crea una reserva.
5. **Dado** que otra persona tiene reservada la sala A de 09:00 a 10:00, **Cuando** intento
   reservar la sala A de 10:09 a 11:09, **Entonces** la solicitud se rechaza con el mensaje
   "La sala ya está reservada en ese horario" porque no deja 10 minutos entre reservas.
6. **Dado** que otra persona tiene reservada la sala A de 09:00 a 10:00, **Cuando** intento
   reservar la sala A de 10:10 a 11:10, **Entonces** la reserva se acepta porque deja exactamente
   10 minutos entre reservas.
7. **Dado** que otra persona tiene reservada la sala A de 09:00 a 10:00, **Cuando** intento
   reservar la sala B de 09:30 a 10:30, **Entonces** la reserva se acepta porque se trata de otra
   sala.

### Edge Cases

- Si dos intervalos de la misma sala coinciden en algún momento, la nueva solicitud se rechaza
  con el mensaje "La sala ya está reservada en ese horario".
- Entre reservas de la misma sala deben quedar al menos 10 minutos libres; exactamente 10 minutos
  se permiten.
- Una reserva de otra sala no impide reservar el mismo horario.

## Requirements

### Functional Requirements

- **FR-001**: El estudiante elige una sala, una hora de inicio y una hora de fin.
- **FR-002**: La hora de fin debe ser posterior a la hora de inicio.
- **FR-003**: No se puede crear una reserva que coincida en el tiempo con otra reserva de la
  misma sala.
- **FR-004**: Si la sala ya está reservada durante cualquier parte del intervalo solicitado, la
  solicitud se rechaza con el mensaje exacto "La sala ya está reservada en ese horario".
- **FR-005**: Deben quedar al menos 10 minutos libres entre reservas consecutivas de la misma
  sala; una separación exacta de 10 minutos se permite.
- **FR-006**: Una reserva de otra sala no impide reservar el mismo horario.

### Key Entities

- **Reserva**: sala, estudiante, inicio y fin.
- **Sala**: identificada por su nombre (Sala A, Sala B, Sala C).

## Success Criteria

- **SC-001**: Un estudiante completa una reserva en menos de 30 segundos.
