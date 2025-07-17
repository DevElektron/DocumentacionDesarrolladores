# 📏 Uso de la Funcion `sanitizeText(input: string)`

## 🎯 ¿Qué hace?

Remover cualquier otro carácter no permitido en la función.

## 🔧 ¿Qué acepta?

Permitir letras (mayúsculas y minúsculas), números, espacios, comas, puntos, guiones, paréntesis y acentos latinos comunes

---

## 🛠️ ¿Cómo implementarla?

### 1. como mandarla a llamar en tu modulo?

Importar en tu componente la funcion asi:

```html
    import { sanitizeText } from 'src/app/shared/functions/sanitize';
```

### 2. Se Implementa en typescript y se entre parentesis de la funcion se coloca el texto a sanitizar, para evitar guardar algun script en la variable.

```html
      const requestData: RegistrardepositoRequest = {
        consecutivo: this.form.get('consecutivo')?.value || 0,
        ncte: this.clienteSel.ncte,
        fcdeposito: convertDateToInt(this.form.get('fechadeposito').value),
        ctabancaria: this.form.get('ctabancaria').value?.nctab,
        transaccion: this.form.get('transaccion').value,
        importe: this.form.get('importe').value,
        observaciones: sanitizeText(this.form.get('descripcion')?.value || ''),
        netid: this.netid
      };
```
