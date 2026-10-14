# Tablero de Mantenimiento Mecánico 2026

Tablero web de indicadores de mantenimiento mecánico (disponibilidad, confiabilidad, TMEF, TMPR), costos, repuestos, SOLPED y alertas de análisis de fallas por planta (P02–P14).

## Estructura

| Archivo | Contenido |
|---|---|
| `index.html` | Interfaz, estilos y lógica de cálculo |
| `data.js` | Datos del tablero **cifrados** (AES-256-GCM), generados con `privado/publicar.js` |

## Acceso por código y roles

El tablero pide un código de acceso. Cada código pertenece a un usuario con un rol, y el rol define qué secciones ve:

| Sección | Contenido |
|---|---|
| `indicadores` | Pestaña «Indicadores» |
| `costos` | Pestaña «Costos, repuestos y alertas» |

Los datos de costos van cifrados con una llave que solo reciben los roles con la sección `costos` o con administración; sin un código válido, `data.js` no se puede leer. Los códigos son de 4 dígitos.

## Módulo de administración

Los roles con el permiso «Administración» ven una pestaña extra donde se puede:

- Agregar, renombrar o eliminar usuarios y asignarles un rol.
- Cambiar el código de cualquier usuario o generar uno nuevo al azar.
- Crear roles y marcar qué ve cada uno: Indicadores, Costos, botón Excel y Administración.

El botón **Guardar cambios** genera el `data.js` nuevo y lo publica en GitHub en un solo paso. La primera vez pide un token de GitHub con permiso de escritura (Contents: Read and write) en este repositorio; con «Recordar en este equipo» queda guardado solo en ese navegador. Si la publicación falla, aparece la opción de descargar una copia de `data.js`. La lista de usuarios viaja cifrada dentro de `data.js` y solo la pueden abrir los administradores.

## Uso

Abre el link de GitHub Pages e ingresa tu código. El botón **«Actualizar con nuevo Excel»** (solo administrador) recalcula los indicadores con un libro que tenga las hojas «Datos Entrada OEE» e «IW28»; el archivo se procesa solo en el navegador y no se sube a ningún servidor.

## Actualizar los datos

1. Reemplaza `../privado/datos.js` con los datos nuevos.
2. `node ../privado/publicar.js` (vuelve a cifrar los datos y conserva usuarios y permisos).
3. `git add data.js && git commit -m "Datos al <fecha>" && git push`

`../privado/llaves.json` guarda las llaves de cifrado: sin ese archivo no se pueden actualizar los datos conservando los usuarios. `../privado/roles.json` solo se usa la primera vez.

## Metas «Bueno»

TMEF ≥ 16 h · TMPR ≤ 20 min · Disponibilidad ≥ 98,5 % · Confiabilidad ≥ 98 %
