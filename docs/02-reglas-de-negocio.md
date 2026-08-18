# Reglas de negocio de CoreBank

## 1. Introduccion

Este documento define las reglas de negocio que governan el comportamiento de CoreBank. Constituyen la fuente de verdad del dominio y cualquier decision de diseno posterior debe estar alineada con ellas.

Las reglas estan agrupadas por area funcional. Cada regla tiene un identificador numerico para facilitar referencias cruzadas en otros documentos del proyecto.

## 2. Reglas del Cliente

1. Todo cliente tiene un cliente_id generado internamente, no reutilizable.
2. El documento de identidad (cedula, RNC, pasaporte) es unico por tipo de documento.
3. Un cliente es persona fisica o juridica, mediante discriminador tipo_cliente.
4. Los datos especificos de cada tipo se almacenan en tablas satelite.
5. Un cliente puede estar activo o inactivo. El historial se preserva.
6. Un cliente inactivo no puede abrir cuentas nuevas ni realizar operaciones.
7. Un cliente tiene una sucursal de origen, que no cambia.

## 3. Reglas de la Cuenta

8. Una cuenta pertenece a exactamente un cliente titular en el MVP.
9. Una cuenta tiene un numero de cuenta generado internamente, unico en el sistema.
10. Una cuenta pertenece a un tipo de cuenta (ahorro, corriente).
11. Las reglas del producto se copian en la cuenta al momento de apertura (snapshot).
12. Una cuenta tiene un saldo actual materializado, mantenido transaccionalmente.
13. Una cuenta tiene una sucursal de apertura que no cambia.
14. Una cuenta tiene una moneda (asumimos DOP en el MVP).
15. Una cuenta puede estar activa, bloqueada, cerrada. El estado cerrado es terminal.
16. Una cuenta puede tener un tipo de bloqueo (DEBITO, CREDITO, TOTAL) o ninguno.
17. Una cuenta bloqueada no permite operaciones que afecten su saldo segun el tipo de bloqueo.

## 4. Reglas de la Transaccion

18. Toda operacion que afecte saldo se registra como una transaccion.
19. El saldo no se modifica directamente fuera del contexto de una transaccion.
20. Cada transaccion tiene un tipo (deposito, retiro, transferencia-debito, transferencia-credito, comision, reversion).
21. Cada transaccion tiene un monto positivo; el signo lo define el tipo.
22. Cada transaccion tiene un origen (ventanilla, cajero, sistema, transferencia).
23. Cada transaccion tiene un estado (APLICADA, REVERTIDA, FALLIDA).
24. Los datos economicos y de identidad de una transaccion son inmutables desde su aplicacion.
25. El campo estado puede transicionar de APLICADA a REVERTIDA exclusivamente por una reversion valida.
26. Una reversion genera una nueva transaccion que referencia a la original.

## 5. Reglas de la Transferencia

27. Una transferencia tiene un transferencia_id propio.
28. Una transferencia genera exactamente dos transacciones (debito y credito) que comparten el transferencia_id.
29. Las dos transacciones se ejecutan en una sola transaccion de base de datos (atomicidad).
30. Si alguna validacion falla, la transferencia falla completa sin generar transacciones.
31. Los estados de una transferencia son COMPLETADA, FALLIDA, o REVERTIDA.
32. Una transferencia revertida es una nueva transferencia que referencia a la original.
33. Una transferencia COMPLETADA solo puede ser revertida una vez.

## 6. Reglas de Auditoria

34. CoreBank implementa auditoria en dos capas: aplicacion y base de datos.
35. La auditoria de aplicacion registra operaciones de negocio con contexto.
36. La auditoria de base de datos registra cambios a nivel de fila.
37. Ambas auditorias son append-only.
38. La auditoria de BD constituye la ultima capa de trazabilidad dentro de la base de datos.
39. La tabla de auditoria es accesible solo para el rol AUDITOR.

## 7. Reglas de Usuarios y Seguridad

40. Un usuario del sistema pertenece a un rol y a una sucursal.
41. Roles predefinidos: CAJERO, SUPERVISOR, AUDITOR, ADMIN.
42. Un usuario solo puede operar cuentas de su sucursal, excepto ADMIN y AUDITOR.
43. Una cuenta solo puede ser operada por usuarios de su sucursal de apertura.
44. Un usuario solo puede revertir una transaccion una vez.

## 8. Indice de reglas por area

| Area | Reglas |
|------|--------|
| Cliente | 1, 2, 3, 4, 5, 6, 7 |
| Cuenta | 8, 9, 10, 11, 12, 13, 14, 15, 16, 17 |
| Transaccion | 18, 19, 20, 21, 22, 23, 24, 25, 26 |
| Transferencia | 27, 28, 29, 30, 31, 32, 33 |
| Auditoria | 34, 35, 36, 37, 38, 39 |
| Usuarios y seguridad | 40, 41, 42, 43, 44 |
