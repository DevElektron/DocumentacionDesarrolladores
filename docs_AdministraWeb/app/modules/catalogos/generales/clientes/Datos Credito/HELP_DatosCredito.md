# 📦 Módulo: Datos de Crédito
#### 📁 **Código:** `/modules/catalogos/generales/clientes/datos-credito`
#### 💻 **Menú:** Menú > > >  [Ver en QA](http://192.168.2.16:1089/app/catalogos/generales/clientes/datoscredito)

## 📝 Descripción
Este módulo permite gestionar los datos generales de crédito de los clientes, incluyendo configuración de límites de crédito, estados del cliente, firmas autorizadas y referencias bancarias. Controla tanto el alta como la modificación de clientes con validaciones específicas para cada operación.

## 🔐 Seguridad
| Tipo UI | Elemento          | Descripción                    | Rol permitido |
|---------|-------------------|--------------------------------|----------------|
| Botón   | Act. Datos Fiscales      | Realiza ajuste automático de domicilio y nombre      |        |
| Sección   | Firmas   | 	Bloquea/activa visualización de firmas según permiso      |    Gerente CXC    |

## 💼 Políticas Generales
1. Al abrir el formulario, se guarda el estado actual del cliente (CTE:Estado) para comparación posterior.
2. Si se cambia el estado del cliente, se asigna automáticamente la fecha actual al campo CTE:Fc_CamEst = Today()
3. El nombre del cliente se convierte automáticamente a mayúsculas.
4. Los siguientes campos son de solo lectura y no se pueden modificar:
- Días crédito
- Límite crédito
- Bandera exceder límite
- Monto extra autorizado
- Fecha límite de validez
5. La cuenta contable se genera automáticamente si se conecta por Elektron; si es por Norlek, el usuario debe capturarla manualmente.
6. Si se activa la bandera de enviar factura electrónica, el campo correoelectronico se vuelve obligatorio.
7. La sección de firmas solo se muestra si el usuario tiene el permiso correspondiente.
8. Las firmas se pueden previsualizar al hacer clic en la fila correspondiente.

## 🧪 Casos de Prueba

### 1. Alta de Cliente

#### 💼 Operación

* [ ] La fecha de alta se establece automáticamente como el día actual.
* [ ] La fecha de inicio de crédito no se puede ingresar manualmente.
* [ ] El campo Tipo de Pago siempre muestra "Contado" y está bloqueado para modificación.
* [ ] Al presionar Tab en el campo ciudad, se generan automáticamente las referencias bancarias.
* [ ] La clasificación de cobranza se establece automáticamente en "B".
* [ ] El botón "Act. Datos Fiscales" hace el ajuste de domicilio y nombre automaticamente.

#### 🛡️ Validaciones

* [ ] Al terminar de escribir en nombre, se realiza el ajuste (separación) y se coloca en los campos de declaraciones (solo si hay RFC ingresado).
* [ ] Al terminar de escribir en domicilio, se realiza el ajuste (separación) y se colocan en los campos de declaraciones.
* [ ] Al escribir el RFC, se ejecutan ambos procesos: ajuste de domicilio y nombre.
* [ ] El botón "Act. Datos Fiscales" realiza el ajuste completo de domicilio y nombre.

### 2. Validaciones Antes de Guardar

#### 🛡️ Validaciones

* [ ] Si el estado del cliente cambió desde la apertura del formulario, se actualiza CTE:Fc_CamEst con la fecha actual.
* [ ] Si la bandera de pagarés (Bnd_Pagare) está activa, se verifica que existan pagarés registrados en la tabla CTEP.
* [ ] El nombre se convierte a mayúsculas.
* [ ] Se pregunta si es cliente nuevo o cambio de razón social, actualizando BndCteNvo según corresponda.
* [ ] La bandera Bnd_AsigFact siempre se establece en true.
* [ ] Se verifica que el RFC no esté duplicado en la base de datos.
* [ ] La bandera BndBackOrder se establece en true.

### 3. Modificación de Cliente

#### 💼 Operación

* [ ] Se aplican las mismas políticas que en el alta, excepto la inicialización de valores por defecto.
* [ ] Los campos de crédito permanecen de solo lectura.
* [ ] Se mantiene la validación del estado y actualización de CTE:Fc_CamEst.

#### 🛡️ Validaciones

* [ ] No se permiten modificar los campos de configuración de crédito.
* [ ] Se aplican todas las validaciones previas al guardado del alta.

### 4. Estados del Cliente

#### 🛡️ Validaciones

* [ ] Estados válidos:
    - C = Corriente
    - S = Suspendido
    - A = Cancelado
    - J = Jurídico
    - I = Incobrable
    - D = Especiales
    - P = Por saldo
* [ ] Cualquier cambio de estado actualiza CTE:Fc_CamEst.

## 📎 Observaciones adicionales
- **Mensaje de error específico:** Si Bnd_Pagare = true y no hay pagarés registrados: "Tiene la Bandera de Pagarés Activada y NO Tiene Pagarés Registrados. Favor de Desactivar la Bandera de Pagarés o Registrar los Pagarés"

#### Notas Técnicas
* Las referencias bancarias se generan automáticamente basándose en la ciudad
* Los ajustes de nombre/domicilio separan: calle, número interior, número exterior, nombre, apellido paterno, apellido materno
* La conexión Elektron/Norlek determina el comportamiento del campo cuenta contable


> 🗓️ **Fecha de última modificación:** 2026-23-01
> 👤 **Jose Daniel Salazar Briseño**
> 🏷️ **Versión:** 1

