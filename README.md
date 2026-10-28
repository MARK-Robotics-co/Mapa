# Mapa 3D de Racks Ampliado

Mapa 3D interactivo de un almacén de rack selectivo. Se puede girar, acercar, buscar ubicaciones por etiqueta y cambiar la configuración del almacén.

Todo está en un solo archivo, `index.html`, que se abre en cualquier navegador. Necesita internet porque carga el motor 3D (Three.js) desde un CDN.

## Cómo abrirlo

- **Descargado:** abre `index.html` con doble clic.
- **En línea:** si el repositorio tiene GitHub Pages activado (Settings → Pages → rama `main`), el mapa queda en `https://<usuario>.github.io/<repositorio>/`.

## Cómo está armado el almacén

Los bloques van en orden desde los andenes hacia el fondo.

| Elemento | Valor |
|---|---|
| Racks | 58 en total |
| Rack doble | 1 bloque de 2 racks espalda con espalda, junto a los andenes |
| Bloques originales | 9 bloques de 4 racks, de 40 espacios (80 lugares) |
| Bloques nuevos | 5 bloques de 4 racks al fondo, de 43 espacios (86 lugares) |
| Lugares | 2 tarimas por espacio a lo largo |
| Niveles | 5 niveles de 1.6 m (8 m de altura útil) |
| Rack doble, lugares 24 a 80 | 6 niveles de 1.33 m, misma altura total |
| Capacidad total | 22,154 tarimas, todas vacías |

### Bloques nuevos

Los 3 espacios extra de los bloques nuevos sobresalen hacia el extremo opuesto, antes de donde empiezan los racks originales. En esos racks el lugar 01 está en la punta que sobresale.

### Puentes para montacargas doble reach

Los puentes quitan los primeros niveles para dejar 4.8 m de paso libre. Están en la misma posición física en todos los racks, así que el montacargas cruza en línea recta:

| Racks | Lugares de los puentes |
|---|---|
| Rack doble y bloques originales | 27–31 y 54–58 |
| Bloques nuevos | 33–37 y 60–64 |

En el tramo de 6 niveles del rack doble, el puente quita los primeros 4 niveles para mantener los 4.8 m libres.

## Etiquetas

Las etiquetas van en el larguero de la cara que da al pasillo, debajo de cada posición. El formato es una letra seguida de 5 dígitos, `X RR N LL`:

| Parte | Significado | Ejemplo en `E12001` |
|---|---|---|
| `X` | `E` = rack exterior (da al pasillo), `I` = rack interior (detrás) | `E` |
| `RR` | Número de rack según el pasillo | `12` |
| `N` | Nivel menos 1 (el nivel 1 se etiqueta como 0) | `0` |
| `LL` | Lugar | `01` |

Numeración de las caras, empezando por los andenes:

- **Rack doble:** caras **10** (hacia los andenes) y **11**. Solo llevan E, una etiqueta por posición. Por ejemplo, `E10001`.
- **Bloques originales:** caras **12–13, 14–15 … 28–29**.
- **Bloques nuevos:** caras **30–31 … 38–39**.
- **Etiquetas E e I en los bloques de 4:** cada cara lleva dos etiquetas juntas. La E va a la izquierda vista desde el pasillo y la I a la derecha, con el mismo número de cara. Por ejemplo, `E12001` y `I12001`.

En el buscador del mapa se puede escribir una etiqueta (`E30001`, `I12001`) o solo los números (`12001`, que muestra la E y la I).

## Cambios sin tocar código

En el panel izquierdo, sección **Configurar almacén**, se pueden cambiar los bloques, los espacios, los niveles, los bloques nuevos, los puentes y el tramo de 6 niveles. Luego se pulsa **Regenerar**.

> Lo que se cambia en el panel se guarda solo en el navegador de esa persona. Para que un cambio sea para todos, hay que cambiarlo en el código y subirlo al repositorio.

## Cambios en el código

| Qué cambiar | Dónde en `index.html` |
|---|---|
| Configuración por defecto (bloques, espacios, niveles, puentes, bloques nuevos) | `let cfg = {` (aprox. línea 253) |
| Medidas en metros: `MOD_W` (frente del espacio), `DEPTH` (fondo), `LVL_H` (altura de nivel), `BACK_GAP` (separación entre racks pegados), `AISLE` (pasillo) | `const MOD_W = …` (aprox. línea 223) |
| Tamaño de la tarima | `const PAL_W = …` (aprox. línea 224) |
| Primer número de cara de las etiquetas | `const TAG_FIRST = 10;` (aprox. línea 226) |
| Pasillos de cabecera, zona de andenes y espacio al frente | `const HEAD = 4, DOCK = 11, FRONT = 3;` (aprox. línea 369) |
| Número de puertas de andén | `const nDoors = …` (aprox. línea 388) |
| Colores | Bloque `:root {` al inicio del archivo, con su versión para modo oscuro debajo |

**Importante:** si cambias `let cfg = {` y no ves el cambio, es porque tu navegador recuerda la configuración anterior. Cambia `racks3d-grande.cfg.v2` a `v3` en las dos líneas donde aparece, o abre el mapa en una ventana privada.

## Pendiente

- Todas las posiciones están **vacías**. El mapa ya maneja los estados Ocupado, Reservado y Bloqueado, pero todavía no hay forma de cargarlos desde un inventario.
- Las medidas son estándar de rack selectivo. Conviene confirmarlas con el plano real de la nave.
