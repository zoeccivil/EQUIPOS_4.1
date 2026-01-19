# EQUIPOS 5.0 - UI Refactoring Guide

## Arquitectura de Navegación - Sidebar + QStackedWidget

### Cambios Implementados

#### 1. **Estructura de Layout Principal**
```
┌──────────────────────────────────────────────────────┐
│  QMainWindow (EQUIPOS 5.0)                           │
├──────────────┬───────────────────────────────────────┤
│   SIDEBAR    │        QStackedWidget                 │
│   (250px)    │                                       │
│              │                                       │
│  📊 Dashboard│     [Vista Actual]                    │
│  📋 Alquileres│                                       │
│  💰 Gastos   │     - Dashboard                       │
│  💳 Pagos    │     - Registro de Alquileres          │
│              │     - Gastos de Equipos               │
│              │     - Pagos a Operadores              │
│              │                                       │
│  v5.0        │                                       │
└──────────────┴───────────────────────────────────────┘
```

#### 2. **Sidebar Design (Industrial Dark Mode)**
- **Ancho fijo**: 250px
- **Color de fondo**: `#1E1E1E` (bg_surface)
- **Borde derecho**: 1px solid `#333333` (border)
- **Título**: "EQUIPOS 5.0" en azul IBM (`#0F62FE`)
- **Botones de navegación**:
  - Estado normal: Transparente con texto gris
  - Hover: Fondo `#2C2C2C` con texto blanco
  - Activo: Fondo azul IBM (`#0F62FE`) con texto blanco

#### 3. **Preservación de Lógica de Negocio**

##### Referencias Legacy Mantenidas:
```python
# Nuevas propiedades (vistas)
self.dashboard_view
self.registro_view
self.gastos_view
self.pagos_view

# Referencias legacy (compatibilidad)
self.dashboard_tab = self.dashboard_view
self.registro_tab = self.registro_view
self.gastos_tab = self.gastos_view
self.pagos_tab = self.pagos_view
```

##### Métodos Preservados:
- `_cargar_datos_iniciales()` - ✅ Sin cambios
- `_cargar_mapas_y_poblar_tabs()` - ✅ Sin cambios
- `_generar_reporte_detallado_pdf()` - ✅ Sin cambios
- `_generar_reporte_rendimientos_pdf()` - ✅ Sin cambios
- Todos los métodos del menú - ✅ Sin cambios

##### Imports Preservados:
- ✅ `FirebaseManager`
- ✅ `BackupManager`
- ✅ `StorageManager`
- ✅ `DashboardTab`
- ✅ `RegistroAlquileresTab`
- ✅ `TabGastosEquipos`
- ✅ `TabPagosOperadores`

#### 4. **QMenuBar**
- Mantenido completamente funcional
- Aplicado estilo Dark Mode vía `AppTheme.get_stylesheet()`
- Todos los menús siguen funcionando:
  - Archivo
  - Gestión
  - Reportes
  - Configuración

#### 5. **Navegación**

##### Método `_cambiar_vista(index)`:
```python
def _cambiar_vista(self, index: int):
    """Cambia la vista actual y actualiza botones"""
    self.stackedWidget.setCurrentIndex(index)
    
    # Actualizar estado de botones
    for i, btn in enumerate(self.nav_buttons):
        btn.setChecked(i == index)
```

##### Índices de vistas:
- 0: Dashboard
- 1: Registro de Alquileres
- 2: Gastos de Equipos
- 3: Pagos a Operadores

## Componentes Reutilizables (AppTheme)

### 1. **KPICard**
Tarjeta para mostrar métricas con:
- Título (texto secundario, uppercase)
- Valor (fuente monospace grande)
- Cambio opcional (verde/rojo)

### 2. **ModernTable**
Tabla pre-configurada con:
- Bordes redondeados
- Grid horizontal solo en headers
- Filas de altura mínima 45px
- Sin numeración de filas

### 3. **StatusBadge**
Etiqueta de estado con colores:
- Success: Verde (`#24A148`)
- Warning: Amarillo (`#F1C21B`)
- Danger: Rojo (`#DA1E28`)

### 4. **ModernButton**
Botón con estilos predefinidos:
- Primary: Azul IBM
- Secondary: Transparente con borde
- Success/Danger: Colores semánticos

## Testing

Para verificar que todo funciona correctamente:

1. La aplicación debe iniciar sin errores
2. El sidebar debe ser visible a la izquierda
3. Los botones de navegación deben cambiar de vista
4. El botón activo debe tener fondo azul
5. El QMenuBar debe seguir funcionando
6. Los mapas deben cargarse correctamente
7. Los reportes deben generarse sin problemas

## Screenshots Esperados

La UI debe verse similar a herramientas modernas como Linear o software financiero:
- Fondo oscuro (`#121212`)
- Contraste alto para legibilidad
- Azul IBM como color primario
- Tipografía clara y moderna
- Espaciado generoso entre elementos
