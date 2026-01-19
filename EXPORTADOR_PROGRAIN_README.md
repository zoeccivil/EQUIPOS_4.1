# 📄 Exportador PROGRAIN 5.0 - Documentación

## 🎯 Descripción General

El **Exportador PROGRAIN 5.0** es una nueva funcionalidad integrada en EQUIPOS 4.0 que permite exportar transacciones (gastos e ingresos) a un archivo Excel (.xlsx) compatible con el sistema contable PROGRAIN 5.0.

## ✨ Características Principales

- ✅ **Exportación de Gastos e Ingresos**: Puede incluir ambos tipos de transacciones o solo uno
- ✅ **Filtros Avanzados**: Por año y mes (o todo el año)
- ✅ **Vista Previa en Tiempo Real**: Visualice las transacciones antes de exportar
- ✅ **Estadísticas Automáticas**: Total de débitos, créditos y balance
- ✅ **Validación de Formato**: Verifica que los datos cumplan con PROGRAIN 5.0
- ✅ **Formato Excel Profesional**: Con estilos, colores y formatos numéricos

## 🚀 Cómo Usar

### 1. Acceder al Exportador

Hay tres formas de abrir el exportador:

1. **Desde el Menú**: `Reportes` → `💾 Exportar a PROGRAIN 5.0`
2. **Atajo de Teclado**: Presiona `Ctrl+Shift+P`
3. **Desde la barra de menú superior**

### 2. Configurar Filtros

En la sección **"FILTROS DE EXPORTACIÓN"**:

- **Año**: Seleccione el año de las transacciones a exportar
- **Mes**: 
  - Seleccione "Todos" para exportar todo el año
  - O seleccione un mes específico (Enero, Febrero, etc.)
- **Incluir Gastos**: Marque para incluir gastos en la exportación
- **Incluir Ingresos**: Marque para incluir ingresos (alquileres) en la exportación

> **Nota**: Debe marcar al menos una opción (Gastos o Ingresos)

### 3. Cargar Vista Previa

Haga clic en el botón **"🔍 Cargar Vista Previa"** para ver las transacciones que se exportarán.

La tabla mostrará:
- **Fecha**: Fecha de la transacción
- **Concepto**: Categoría del gasto o "INGRESO ALQUILER"
- **Detalle**: Descripción completa con equipo, cliente y proyecto
- **Débito**: Montos de gastos (en rojo)
- **Crédito**: Montos de ingresos (en verde)

### 4. Revisar Estadísticas

En la sección **"RESUMEN DE EXPORTACIÓN"** verá:
- Total de transacciones
- Rango de fechas
- Total de gastos (débitos)
- Total de ingresos (créditos)
- **Balance**: Diferencia entre créditos y débitos

### 5. Validar Formato (Opcional)

Haga clic en **"✅ Validar Formato"** para verificar que los datos cumplen con las especificaciones de PROGRAIN 5.0.

La validación verifica:
- ✅ Presencia de todas las columnas requeridas
- ✅ Fechas válidas
- ✅ Montos numéricos positivos
- ✅ Solo débito O crédito tiene valor (no ambos)

### 6. Exportar a Excel

1. Haga clic en **"📄 Exportar a Excel"**
2. Elija la ubicación y nombre del archivo
   - Nombre sugerido: `PROGRAIN_Transacciones_{Año}_{Mes}_{Timestamp}.xlsx`
3. Haga clic en "Guardar"
4. Confirme si desea abrir el archivo automáticamente

## 📋 Formato del Archivo Excel

El archivo exportado cumple con las siguientes especificaciones:

### Columnas

| Columna | Tipo | Descripción | Ejemplo |
|---------|------|-------------|---------|
| **Fecha** | datetime | Fecha de la transacción | 2025-01-15 |
| **Concepto** | texto | Categoría o tipo de transacción | GASTO COMBUSTIBLE |
| **Detalle** | texto | Descripción completa | [Excavadora CAT 320] Diesel tanque lleno |
| **Débito** | número | Monto de gasto (0 si es ingreso) | 15000.50 |
| **Crédito** | número | Monto de ingreso (0 si es gasto) | 0.00 |

### Reglas de Negocio

- **Gastos**: Débito > 0, Crédito = 0.00
- **Ingresos**: Crédito > 0, Débito = 0.00
- **Ordenamiento**: Por fecha ascendente
- **Formato de Montos**: Sin símbolos de moneda, con 2 decimales
- **Formato de Fecha**: YYYY-MM-DD

### Estilos Visuales

- **Encabezados**: Fondo azul (#366092), texto blanco, negrita
- **Montos**: Alineación derecha, separador de miles, 2 decimales
- **Fechas**: Alineación centrada, formato YYYY-MM-DD
- **Anchos de Columna**: Optimizados para legibilidad

## 🔄 Mapeo de Datos

### Para Gastos

```
Fecha    ← gasto.fecha
Concepto ← categorias[gasto.categoria_id]
Detalle  ← "[{equipo}] {descripcion} ({comentario})"
Débito   ← gasto.monto
Crédito  ← 0.00
```

### Para Ingresos (Alquileres)

```
Fecha    ← alquiler.fecha
Concepto ← "INGRESO ALQUILER" (fijo)
Detalle  ← "{equipo} - {cliente} - {proyecto}"
Débito   ← 0.00
Crédito  ← alquiler.monto
```

## ⚙️ Requisitos Técnicos

### Dependencias Python

El exportador requiere las siguientes bibliotecas:

```bash
pip install pandas openpyxl
```

Estas dependencias ya están instaladas si siguió la instalación estándar de EQUIPOS 4.0.

### Versiones Probadas

- Python 3.12+
- pandas 2.3.3+
- openpyxl 3.1.5+
- PyQt6 (ya incluido en EQUIPOS 4.0)

## 🐛 Solución de Problemas

### "Datos no disponibles"

**Problema**: Al abrir el exportador aparece un mensaje de que los datos no están disponibles.

**Solución**: Espere a que la aplicación termine de cargar completamente. Los mapas de datos deben cargarse desde Firebase antes de usar el exportador.

---

### "No se encontraron transacciones"

**Problema**: La vista previa no muestra ninguna transacción.

**Solución**: 
- Verifique que existan transacciones para el período seleccionado
- Asegúrese de haber marcado al menos un tipo (Gastos o Ingresos)
- Revise los filtros de fecha (año y mes)

---

### Error al exportar

**Problema**: La exportación falla con un error.

**Solución**:
1. Verifique que tiene permisos de escritura en la carpeta de destino
2. Cierre el archivo Excel si está abierto
3. Revise los logs de la aplicación para más detalles
4. Intente con una ruta diferente

---

### Formato incorrecto en PROGRAIN

**Problema**: PROGRAIN rechaza el archivo importado.

**Solución**:
1. Use el botón "✅ Validar Formato" antes de exportar
2. Asegúrese de que las transacciones tengan fechas válidas
3. Verifique que los montos sean positivos
4. Confirme que el archivo no fue modificado manualmente después de exportar

## 📊 Ejemplos de Uso

### Caso 1: Exportar Todos los Gastos de Enero 2025

1. Año: **2025**
2. Mes: **Enero**
3. ✅ Incluir Gastos
4. ❌ Incluir Ingresos
5. Cargar Vista Previa → Exportar

### Caso 2: Exportar Todo el Año 2024 (Gastos e Ingresos)

1. Año: **2024**
2. Mes: **Todos**
3. ✅ Incluir Gastos
4. ✅ Incluir Ingresos
5. Cargar Vista Previa → Exportar

### Caso 3: Exportar Solo Ingresos de Q1 2025

Para exportar múltiples meses, debe hacer 3 exportaciones separadas:
- Enero 2025 (solo ingresos)
- Febrero 2025 (solo ingresos)
- Marzo 2025 (solo ingresos)

O alternativamente:
- Usar "Todos" y filtrar en PROGRAIN

## 📝 Notas Importantes

1. **No Modifique el Archivo Manualmente**: El archivo Excel debe importarse a PROGRAIN tal como fue exportado
2. **Validación Previa**: Siempre valide el formato antes de enviar a PROGRAIN
3. **Respaldo**: Guarde copias de los archivos exportados para auditoría
4. **Proyectos Opcionales**: Si no tiene proyectos configurados, el detalle de ingresos no incluirá esa información
5. **Moneda Configurable**: La moneda mostrada en estadísticas se toma de la configuración de la app

## 🔐 Seguridad y Privacidad

- ✅ Los datos se exportan directamente desde Firebase
- ✅ No se envía información a servidores externos
- ✅ Los archivos se guardan localmente en su equipo
- ✅ Solo se incluyen los datos dentro del rango de fechas seleccionado

## 📧 Soporte

Si encuentra problemas o necesita asistencia:

1. Revise los logs de la aplicación en la consola
2. Consulte esta documentación
3. Contacte al administrador del sistema
4. Reporte el problema con capturas de pantalla y logs

---

**Versión del Exportador**: 1.0.0  
**Compatible con**: EQUIPOS 4.0, PROGRAIN 5.0  
**Última actualización**: Enero 2025
