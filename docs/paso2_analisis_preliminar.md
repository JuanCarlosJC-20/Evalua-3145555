# Paso 2: Lectura Guiada del SQL - Análisis Preliminar

**Fecha:** 2026-04-14  
**Objetivo:** Mapear bloques de tablas y relaciones para identificar dominios.

---

## DOMINIO 1: Geografía y Referencia

**Propósito:** Datos de localización y catálogos base del sistema.

**Tablas:**
- `time_zone`
- `continent`
- `country`
- `state_province`
- `city`
- `district`
- `address`
- `currency`

**Relaciones clave (FK):**
- `state_province.country_id` → `country.country_id`
- `city.state_province_id` → `state_province.state_province_id`
- `district.city_id` → `city.city_id`
- `address.district_id` → `district.district_id`

**Índices principales:**
- `idx_country_continent_id`
- `idx_state_country_id`
- `idx_city_state_id`
- `idx_district_city_id`

**Restricciones clave:**
- `uq_country_name` - nombres únicos
- `ck_address_longitude` - rango de longitud -180 a 180

---

## DOMINIO 2: Aerolínea

**Propósito:** Información de la aerolínea.

**Tablas:**
- `airline`

**Relaciones clave:** Ninguna FK interna (tabla independiente).

**Restricciones:**
- `ck_airline_icao_len` - código ICAO de 3 caracteres

---

## DOMINIO 3: Identidad

**Propósito:** Datos de personas y documentación.

**Tablas:**
- `person_type`
- `document_type`
- `contact_type`
- `person`
- `person_document`
- `person_contact`

**Relaciones clave (FK):**
- `person.person_type_id` → `person_type.person_type_id`
- `person.nationality_country_id` → `country.country_id`
- `person_document.person_id` → `person.person_id`
- `person_document.document_type_id` → `document_type.document_type_id`
- `person_contact.person_id` → `person.person_id`
- `person_contact.contact_type_id` → `contact_type.contact_type_id`

**Índices principales:**
- `idx_person_person_type_id`
- `idx_person_document_person_id`
- `idx_person_contact_person_id`

**Restricciones:**
- `ck_person_gender` - valores permitidos: F, M, X
- `ck_person_document_dates` - expires_on >= issued_on

---

## DOMINIO 4: Seguridad

**Propósito:** Control de acceso, autorización y auditoría.

**Tablas:**
- `user_status`
- `security_role`
- `security_permission`
- `user_account`
- `user_role`
- `role_permission`

**Relaciones clave (FK):**
- `user_account.user_status_id` → `user_status.user_status_id`
- `user_role.user_account_id` → `user_account.user_account_id`
- `user_role.security_role_id` → `security_role.security_role_id`
- `role_permission.security_role_id` → `security_role.security_role_id`
- `role_permission.security_permission_id` → `security_permission.security_permission_id`

**Índices principales:**
- `idx_user_account_status_id`
- `idx_user_role_user_account_id`
- `idx_role_permission_role_id`

**Restricciones:**
- `uq_user_account_username` - usernames únicos
- `uq_user_role` - combinación (user_account, role) única

---

## DOMINIO 5: Clientes y Fidelización

**Propósito:** Gestión de clientes y programa de lealtad.

**Tablas:**
- `customer_category`
- `benefit_type`
- `loyalty_program`
- `loyalty_tier`
- `customer`
- `loyalty_account`
- `loyalty_account_tier`
- `miles_transaction`
- `customer_benefit`

**Relaciones clave (FK):**
- `customer.airline_id` → `airline.airline_id`
- `customer.person_id` → `person.person_id`
- `loyalty_program.airline_id` → `airline.airline_id`
- `loyalty_account.customer_id` → `customer.customer_id`
- `loyalty_account.loyalty_program_id` → `loyalty_program.loyalty_program_id`
- `loyalty_account_tier.loyalty_account_id` → `loyalty_account.loyalty_account_id`
- `loyalty_account_tier.loyalty_tier_id` → `loyalty_tier.loyalty_tier_id`
- `miles_transaction.loyalty_account_id` → `loyalty_account.loyalty_account_id`
- `customer_benefit.customer_id` → `customer.customer_id`
- `customer_benefit.benefit_type_id` → `benefit_type.benefit_type_id`

**Índices principales:**
- `idx_loyalty_account_customer_id`
- `idx_miles_transaction_account_id`

**Restricciones:**
- `ck_loyalty_tier_required_miles` - required_miles >= 0
- `ck_miles_delta_non_zero` - miles_delta <> 0

---

## DOMINIO 6: Aeropuertos

**Propósito:** Datos de aeropuertos, terminales, puertas y pistas.

**Tablas:**
- `airport`
- `terminal`
- `boarding_gate`
- `runway`
- `airport_regulation`

**Relaciones clave (FK):**
- `airport.address_id` → `address.address_id`
- `airport.city_id` → `city.city_id`
- `terminal.airport_id` → `airport.airport_id`
- `boarding_gate.terminal_id` → `terminal.terminal_id`
- `runway.airport_id` → `airport.airport_id`
- `airport_regulation.airport_id` → `airport.airport_id`

**Índices principales:**
- `idx_airport_address_id`
- `idx_terminal_airport_id`
- `idx_boarding_gate_terminal_id`

**Restricciones:**
- `ck_airport_icao_len` - código ICAO de 4 caracteres
- `ck_runway_length` - length_meters > 0

---

## DOMINIO 7: Aeronaves

**Propósito:** Flota de aeronaves, modelos y mantenimiento.

**Tablas:**
- `aircraft_manufacturer`
- `aircraft_model`
- `cabin_class`
- `aircraft`
- `aircraft_cabin`
- `aircraft_seat`
- `maintenance_provider`
- `maintenance_type`
- `maintenance_event`

**Relaciones clave (FK):**
- `aircraft_model.aircraft_manufacturer_id` → `aircraft_manufacturer.aircraft_manufacturer_id`
- `aircraft.airline_id` → `airline.airline_id`
- `aircraft.aircraft_model_id` → `aircraft_model.aircraft_model_id`
- `aircraft_cabin.aircraft_id` → `aircraft.aircraft_id`
- `aircraft_cabin.cabin_class_id` → `cabin_class.cabin_class_id`
- `aircraft_seat.aircraft_cabin_id` → `aircraft_cabin.aircraft_cabin_id`
- `maintenance_event.aircraft_id` → `aircraft.aircraft_id`
- `maintenance_event.maintenance_type_id` → `maintenance_type.maintenance_type_id`
- `maintenance_event.maintenance_provider_id` → `maintenance_provider.maintenance_provider_id`

**Índices principales:**
- `idx_aircraft_airline_id`
- `idx_aircraft_cabin_aircraft_id`
- `idx_aircraft_seat_cabin_id`

**Restricciones:**
- `ck_aircraft_model_range` - max_range_km > 0
- `ck_maintenance_event_dates` - completed_at >= started_at

---

## DOMINIO 8: Operaciones de Vuelo

**Propósito:** Información de vuelos, segmentos y retrasos.

**Tablas:**
- `flight_status`
- `delay_reason_type`
- `flight`
- `flight_segment`
- `flight_delay`

**Relaciones clave (FK):**
- `flight.airline_id` → `airline.airline_id`
- `flight.aircraft_id` → `aircraft.aircraft_id`
- `flight.flight_status_id` → `flight_status.flight_status_id`
- `flight_segment.flight_id` → `flight.flight_id`
- `flight_segment.origin_airport_id` → `airport.airport_id`
- `flight_segment.destination_airport_id` → `airport.airport_id`
- `flight_delay.flight_id` → `flight.flight_id`
- `flight_delay.delay_reason_type_id` → `delay_reason_type.delay_reason_type_id`

**Índices principales:**
- `idx_flight_aircraft_id`
- `idx_flight_service_date`
- `idx_flight_segment_origin_airport_id`

**Restricciones:**
- `ck_flight_delay_minutes` - delay_minutes > 0
- `uq_flight_instance` - (airline_id, flight_number, service_date) única

---

## DOMINIO 9: Ventas, Reservas y Ticketing

**Propósito:** Flujo comercial: reservas, venta de tickets, tarifas.

**Tablas:**
- `reservation_status`
- `sale_channel`
- `fare_class`
- `fare`
- `ticket_status`
- `reservation`
- `reservation_passenger`
- `sale`
- `ticket`
- `ticket_segment`
- `seat_assignment`
- `baggage`

**Relaciones clave (FK):**
- `reservation.customer_id` → `customer.customer_id`
- `reservation.flight_id` → `flight.flight_id`
- `reservation.reservation_status_id` → `reservation_status.reservation_status_id`
- `reservation_passenger.reservation_id` → `reservation.reservation_id`
- `reservation_passenger.person_id` → `person.person_id`
- `sale.customer_id` → `customer.customer_id`
- `sale.sale_channel_id` → `sale_channel.sale_channel_id`
- `ticket.sale_id` → `sale.sale_id`
- `ticket.ticket_status_id` → `ticket_status.ticket_status_id`
- `ticket_segment.ticket_id` → `ticket.ticket_id`
- `ticket_segment.flight_segment_id` → `flight_segment.flight_segment_id`
- `ticket_segment.fare_id` → `fare.fare_id`
- `seat_assignment.ticket_segment_id` → `ticket_segment.ticket_segment_id`
- `seat_assignment.aircraft_seat_id` → `aircraft_seat.aircraft_seat_id`
- `baggage.ticket_id` → `ticket.ticket_id`

**Índices principales:**
- Múltiples indexados para performance

**Restricciones:**
- `ck_fare_validity` - valid_to >= valid_from
- `ck_baggage_weight` - weight_kg > 0
- `uq_flight_instance` - instancias de vuelo únicas

---

## DOMINIO 10: Abordaje (Boarding)

**Propósito:** Proceso de check-in y embarque.

**Tablas:**
- `boarding_group`
- `check_in_status`
- `check_in`
- `boarding_pass`
- `boarding_validation`

**Relaciones clave (FK):**
- `check_in.ticket_segment_id` → `ticket_segment.ticket_segment_id`
- `check_in.check_in_status_id` → `check_in_status.check_in_status_id`
- `boarding_pass.check_in_id` → `check_in.check_in_id`
- `boarding_pass.boarding_group_id` → `boarding_group.boarding_group_id`
- `boarding_pass.boarding_gate_id` → `boarding_gate.boarding_gate_id`
- `boarding_validation.boarding_pass_id` → `boarding_pass.boarding_pass_id`

**Restricciones:**
- `ck_boarding_validation_result` - valores: APPROVED, REJECTED, MANUAL_REVIEW
- `uq_boarding_pass_barcode` - códigos de barras únicos

---

## DOMINIO 11: Pagos

**Propósito:** Procesamiento de pagos.

**Tablas:**
- `payment_status`
- `payment_method`
- `payment`
- `payment_transaction`
- `refund`

**Relaciones clave (FK):**
- `payment.sale_id` → `sale.sale_id`
- `payment.payment_status_id` → `payment_status.payment_status_id`
- `payment.payment_method_id` → `payment_method.payment_method_id`
- `payment_transaction.payment_id` → `payment.payment_id`
- `refund.payment_transaction_id` → `payment_transaction.payment_transaction_id`

**Restricciones:**
- `ck_payment_amount` - amount > 0
- `ck_refund_dates` - processed_at >= requested_at

---

## DOMINIO 12: Facturación

**Propósito:** Gestión de facturas, impuestos y conversión de moneda.

**Tablas:**
- `tax`
- `exchange_rate`
- `invoice_status`
- `invoice`
- `invoice_line`

**Relaciones clave (FK):**
- `invoice.sale_id` → `sale.sale_id`
- `invoice.invoice_status_id` → `invoice_status.invoice_status_id`
- `invoice.currency_id` → `currency.currency_id`
- `invoice_line.invoice_id` → `invoice.invoice_id`

**Restricciones:**
- `ck_tax_dates` - effective_to >= effective_from
- `ck_invoice_dates` - due_at >= issued_at

---

## Resumen de Dependencias Entre Dominios

```
Geografía y Referencia (BASE)
    ↓
Aerolínea
    ↓
Identidad → Seguridad
    ↓
Clientes y Fidelización
    ↓
Aeropuertos ← Geografía
Aeronaves ← Aerolínea
    ↓
Operaciones de Vuelo (intersecta con Aeropuertos + Aeronaves)
    ↓
Ventas/Reservas/Ticketing ← Clientes + Operaciones
    ↓
Abordaje ← Ticketing
    ↓
Pagos ← Ticketing
    ↓
Facturación ← Pagos + Ticketing
```

---

## Orden Recomendado de Migraciones (Liquibase)

1. **001-referencia.xml** - Geografía, monedas, tipos básicos
2. **002-aerolínea.xml** - Datos de aerolínea
3. **003-identidad.xml** - Personas, documentos
4. **004-seguridad.xml** - Roles, permisos, usuarios
5. **005-clientes-fidelización.xml** - Clientes, lealtad
6. **006-aeropuertos.xml** - Aeropuertos, terminales, puertas
7. **007-aeronaves.xml** - Flota, cabinas, mantenimiento
8. **008-operaciones-vuelo.xml** - Vuelos, segmentos, retrasos
9. **009-ventas-reservas.xml** - Reservas, tarifas, ticketing
10. **010-abordaje.xml** - Check-in, boarding
11. **011-pagos.xml** - Pagos, reembolsos
12. **012-facturación.xml** - Facturas, impuestos, cambios

