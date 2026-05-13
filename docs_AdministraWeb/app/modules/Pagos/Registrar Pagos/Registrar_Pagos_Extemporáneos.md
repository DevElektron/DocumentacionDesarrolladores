# 📦 Módulo: Registrar Pagos Extemporáneos
#### 📁 **Código:** `src\app\modules\controlPagosCorte\registrar-pagos-factura`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089//app/ctrl-pagos/registrar-pagos-extemporaneos)

## 📝 Descripción

Este módulo muestra la pantalla correspondiente a **Registrar Pagos Extemporáneos**, cuyo objetivo es el mismo que [_**Registrar Pagos Facturas**_](https://github.com/DevElektron/DocumentacionDesarrolladores/blob/main/docs_AdministraWeb/app/modules/Pagos/Registrar%20Pagos/Registrar_Pagos_Factura.md), más con la diferencia de que los usuarios escogen el corte de caja para realizar los pagos, a través de un buscador en la esquina superior izquierda; ese buscador de cortes de caja está configurado para listar los cortes del almacén del usuario, evitando ver otros corte de los almacenes que no sean el suyo.

## 🔐 Seguridad

| Tipo UI | Elemento                                   | Descripción                                                                        | Rol permitido                 |
|---------|--------------------------------------------|------------------------------------------------------------------------------------|-------------------------------|
| ventana | Mostrar opción principal                   | Permite mostrar la opción en el menú principal.                                    | Coordinador/a                 |
| botón   | Aceptar (realizar pagos)                   | Confirma la información capturada para pagar la factura.                           | Coordinador/a                 |
| botón   | Guardar (registrar cheque protegido)       | Guarda la información capturada en la ventana de Registro de Cheques Protegidos.   | Coordinador/a                 |
| botón   | Aceptar (leer código de barras de N.A.*)   | Acepta el código de barras ingresado en la ventana de solicitud de Notas de Abono. | Coordinador/a                 |

## 🧪 Casos de Prueba

### 1. No carga la ruta de acceso con rol no permitido

#### 💼 Operación

- [ ] 1. Entra al sistema con las credenciales de un usuario que NO tenga el rol de `Coordinador/a`.
- [ ] 2. Al cargar el sistema, en el menú lateral principal, no habrá la opción `Pagos > Registrar Pagos Extemporaneos`.

#### 🛡️ Validaciones

- [ ] No se encuentra `Pagos > Registrar Pagos Extemporaneos` en el menú al cargar sistema.

### 2. Carga la ruta de acceso con rol permitido

#### 💼 Operación

- [ ] 1. Entra al sistema con las credenciales de un usuario que SÍ tenga el rol de `Coordinador/a`.
- [ ] 2. Al cargar el sistema, en el menú lateral principal, mostrará la opción `Pagos > Extemporaneos`.

#### 🛡️ Validaciones

- [ ] `Pagos > Registrar Pagos Extemporaneos` en el menú al cargar sistema.

### 3. Buscar corte inicial

#### 💼 Operación

- [ ] 1. Entra al sistema con las credenciales de un usuario que SÍ tenga el rol de `Coordinador/a`.
- [ ] 2. Al cargar el sistema, en el menú lateral principal, mostrará la opción `Pagos > Extemporaneos`, da clic en ella.
- [ ] 3. Se cargará la misma pantala de `Registrar Pagos Facturas` pero con 3 diferencias:
  1. Un mensaje inicial: _Busca el corte de caja por turno o descripción del almacén en la esquina superior izquierda_.
  2. Título de la pantalla: _Registrar Pagos Extemporaneos_.
  3. Buscador de cortes de cajas: En lugar de _CORTE {Número de Turno}_, se mostrará un buscador de cortes de caja, configurado para buscar sólo los cortes pertenecientes al almacén del usuario que inició sesión, puedes buscar por el número de Turno/Corte o la descripción del almacén, ej. _Si el usuario tiene en su configuración el almacén 24, puedes buscar **PACHUCA** para que te liste todos los cortes pertenecientes a la descripción, si intentas buscar cortes de LEÓN, no te mostrará nada_.
- [ ] 4. Sin buscar un corte, intenta buscar una factura, no podrás, el buscador de facturas que dice _CÓDIGO DE BARRAS_ estará bloqueado (sin poder escribir), ya que no has seleccionado un corte.
- [ ] 5. Selecciona un corte de caja, y ya podrá buscar facturas y realizar pagos a la misma.

#### 🛡️ Validaciones

- [ ] `Pagos > Registrar Pagos Extemporaneos` en el menú al cargar sistema.
- [ ] Diferencias de __ y esta pantalla notadas como se menciona.
- [ ] Buscador de facturas bloqueado si no has seleccionado un corte.
- [ ] Buscador de facturas habilitado si has seleccionado un corte.

> NOTA: Se pueden hacer las pruebas de [_**Registrar Pagos Facturas**_](https://github.com/DevElektron/DocumentacionDesarrolladores/blob/main/docs_AdministraWeb/app/modules/Pagos/Registrar%20Pagos/Registrar_Pagos_Factura.md#-casos-de-prueba), pero ten en cuenta que las partes en donde haga alusión a los roles `Vendedor` y `Vendedor Mostrador` sólo debes sustituirlas por el rol de `Coordonador/a`, así como las 2 pruebas que prácticamente hacen eso en este documento.
