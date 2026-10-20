# Space Hub

Libreria de interfaz (UI) para Roblox escrita en Luau. Te permite crear menus modernos con pestanas, botones, interruptores, sliders, listas, cajas de texto, selector de color y mucho mas, escribiendo muy poco codigo. Incluye temas, busqueda global, perfil del jugador, guardado de configuracion, notificaciones, sistema de keys, aviso de Discord y soporte para movil.

> **Version:** 1.1.0
> **Lenguaje:** Luau (Roblox)
> **Requisitos:** un ejecutor con `loadstring` y `game:HttpGet`. Para guardar configuracion tambien necesita `writefile`, `readfile` e `isfile`.

---

## Indice

1. [Instalacion rapida](#instalacion-rapida)
2. [Vista general de la interfaz](#vista-general-de-la-interfaz)
3. [Pantalla de carga (intro)](#pantalla-de-carga-intro)
4. [Cabecera y botones de ventana](#cabecera-y-botones-de-ventana)
5. [Modo minimizado (pildora)](#modo-minimizado-pildora)
6. [Barra de arrastre y esquina de tamano](#barra-de-arrastre-y-esquina-de-tamano)
7. [Pestanas (categorias)](#pestanas-categorias)
8. [Buscador global](#buscador-global)
9. [Tarjeta de perfil y vista de perfil](#tarjeta-de-perfil-y-vista-de-perfil)
10. [Ajustes integrados](#ajustes-integrados)
11. [Elementos que puedes crear](#elementos-que-puedes-crear)
12. [Notificaciones](#notificaciones)
13. [Sistema de keys](#sistema-de-keys)
14. [Aviso de Discord](#aviso-de-discord)
15. [Temas](#temas)
16. [Guardado de configuracion y Flags](#guardado-de-configuracion-y-flags)
17. [Iconos](#iconos)
18. [Opciones de CreateWindow](#opciones-de-createwindow)
19. [API completa](#api-completa)
20. [Ejemplo minimo](#ejemplo-minimo)
21. [Solucion de problemas](#solucion-de-problemas)

---

## Instalacion rapida

Pega esta linea al inicio de tu guion. Descarga la libreria y te devuelve la tabla `SpaceHub`:

```lua
local SpaceHub = loadstring(game:HttpGet(
	"https://raw.githubusercontent.com/dream14696/Official-space-hub-1.1.0/refs/heads/main/Library%20space%20hub"
))()
```

Que hace cada parte:

- `game:HttpGet(url)` descarga el texto del archivo desde internet.
- `loadstring(texto)` convierte ese texto en una funcion que se puede ejecutar.
- El `()` final ejecuta la funcion, y esta devuelve la libreria, que se guarda en `SpaceHub`.

---

## Vista general de la interfaz

El menu esta formado por estas zonas:

```
+--------------------------------------------------------------+
| [Icono]                                    [Ajustes][-][X]    |  <- Cabecera
+------------------+-------------------------------------------+
| [Buscador]       |  Contenido de la pestana seleccionada     |
| Pestana 1        |  - Section                                |
| Pestana 2        |  - Label / Paragraph                      |
| Pestana 3        |  - Button / Toggle / Slider ...           |
|                  |                                           |
| [Perfil]         |                                      [◢]  |  <- Esquina de tamano
+------------------+-------------------------------------------+
                      ======  <- Barra de arrastre
```

| Zona | Para que sirve |
|---|---|
| Cabecera | Muestra el icono del menu, permite moverlo y contiene los botones de Ajustes, Minimizar y Cerrar. |
| Lista de pestanas | Columna izquierda (o fila superior en modo horizontal) para cambiar de categoria. |
| Buscador | Encuentra cualquier boton, opcion o pestana por nombre. |
| Contenido | Zona principal donde aparecen los elementos de la pestana activa. |
| Tarjeta de perfil | Abajo a la izquierda. Al pulsarla abre tus datos de Roblox. |
| Barra de arrastre | Linea bajo el menu para moverlo de forma comoda. |
| Esquina de tamano | Triangulo abajo a la derecha para cambiar el tamano de la ventana. |

---

## Pantalla de carga (intro)

Al ejecutar el guion aparece un logo en el centro de la pantalla que se desvanece y desaparece antes de mostrar el menu.

- Se controla con `LoadingScreen` (`true` o `false`).
- La duracion se ajusta con `LoadingDuration` (segundos, por defecto `3`).
- Puedes poner tu propio logo con `LoadingLogo = "rbxassetid://ID"`.
- Si el logo principal no carga, usa automaticamente uno de respaldo.

---

## Cabecera y botones de ventana

La franja superior del menu contiene:

- **Icono del menu** (esquina izquierda). Es la imagen de marca de tu hub. Se cambia con `TitleImage`. Si la imagen no carga, se muestra el texto de `Name` en su lugar.
- **Zona de arrastre.** Mantener pulsada la cabecera y mover el raton (o el dedo) desplaza el menu por la pantalla. El menu nunca sale completamente de la pantalla.
- **Boton Ajustes (engranaje).** Abre y cierra la vista de ajustes. Si algun icono no carga, se dibuja un engranaje de respaldo.
- **Boton Minimizar (`-`).** Convierte el menu en una pildora pequena.
- **Boton Cerrar (`x`).** Pide confirmacion antes de cerrar. Si aceptas, la interfaz se desvanece y se destruye por completo (se desconectan todos los eventos).

---

## Modo minimizado (pildora)

Al minimizar, la ventana se reduce a una pildora de 200x44 con animacion. En ese estado:

- **Maximizar (`+`):** restaura el menu a su tamano normal.
- **Mover (`=`):** permite arrastrar la pildora.
- Tambien puedes **tocar la pildora** (un toque corto, sin arrastrar) para restaurarla.
- La pildora se mantiene siempre dentro de la pantalla.

Por codigo: `Window:Minimize(true)` minimiza y `Window:Minimize(false)` restaura.

---

## Barra de arrastre y esquina de tamano

- **Barra de arrastre:** la linea gris bajo el menu. Al arrastrarla se ilumina, crece un poco y se mueve ligeramente siguiendo tu dedo o raton, y mueve toda la ventana. Es la forma mas comoda de mover el menu en movil.
- **Esquina de tamano (`◢`):** abajo a la derecha dentro del menu. Arrastrala para hacer el menu mas grande o mas pequeno.
  - Tamano minimo: **480 x 310**
  - Tamano maximo: **860 x 580**
  - Los limites se ajustan solos si la pantalla es mas pequena.

---

## Pestanas (categorias)

Las pestanas dividen el menu en secciones. Se crean con `Window:CreateTab(nombre, icono)`.

- El segundo parametro (icono) es opcional. Acepta un ID de Roblox o un nombre registrado con `SpaceHub:SetIcons`.
- La pestana activa se resalta con el color de acento del tema.
- La primera pestana que crees se selecciona automaticamente.
- Cada pestana tiene su propio scroll vertical.

### Modos de menu

Hay dos disposiciones, que se cambian en **Ajustes > Menu type** o por codigo:

| Modo | Descripcion |
|---|---|
| **Vertical categories** | Las pestanas van en una columna a la izquierda y el buscador arriba, sobre el contenido. Es el modo por defecto. |
| **Horizontal categories** | Las pestanas van en una fila arriba y el buscador queda en la columna izquierda. |

```lua
Window:SetMenuType("horizontal") -- o "vertical"
```

---

## Buscador global

Busca en **todas las pestanas y en los ajustes** a la vez. Indexa pestanas, botones, toggles, sliders, dropdowns, inputs, keybinds, color pickers, labels y paragraphs.

Como funciona:

1. Escribe en la caja "Search...".
2. Aparece una lista con las coincidencias.
3. Al pulsar un resultado, el menu cambia a la pestana correcta, se desplaza hasta el elemento y lo **resalta con un destello** del color de acento.

Detalles utiles:

- No distingue mayusculas de minusculas.
- Ignora acentos, la `n` con tilde, espacios y simbolos (buscar `configuracion` encuentra `Configuración`).
- Si no hay coincidencias muestra "No results.".
- En modo vertical la lista se despliega bajo la caja y se limpia al elegir un resultado; en modo horizontal ocupa la columna izquierda.

---

## Tarjeta de perfil y vista de perfil

### Tarjeta (abajo a la izquierda)

Muestra tu foto de avatar (retrato), el texto "Welcome" y tu **nombre visible**. Al pulsarla se abre la vista de perfil.

### Vista de perfil

Incluye tu avatar grande, tu nombre visible y tu usuario (`@usuario`), seguidos de una lista de datos. **Cada dato tiene su boton Copy** que lo copia al portapapeles (muestra "Copied!" al lograrlo o "Unavailable" si tu ejecutor no soporta portapapeles).

| Dato | Que muestra |
|---|---|
| Username | Tu nombre de usuario de Roblox. |
| Display name | Tu nombre visible. |
| User ID | Tu identificador numerico. |
| Registration date | Fecha estimada de creacion de la cuenta (dd/mm/aaaa). |
| Account age | Dias que tiene tu cuenta. |
| Membership | Tipo de membresia (por ejemplo Premium o None). |
| Language | Idioma/region (LocaleId). |
| Profile link | Enlace directo a tu perfil de Roblox. |
| Place ID | ID del juego en el que estas. |
| Job ID | ID del servidor actual. |

El boton **`< Back`** te devuelve al menu principal.

---

## Ajustes integrados

Se abren con el engranaje de la cabecera. Esta pagina viene incluida automaticamente en cualquier menu.

### Interface

- **Themes:** bloque desplegable con una cuadricula de 10 temas. Al pulsar uno, todo el menu cambia de color con una animacion suave. Cada boton muestra el nombre del tema con sus propios colores.
- **Menu type:** lista para elegir entre categorias horizontales o verticales.
- **Toggle interface:** keybind para elegir la tecla que muestra u oculta el menu (por defecto `RightShift`).

### Configuration

- **Save configuration:** guarda en un archivo el valor de todos los elementos que tengan `Flag`.
- **Load configuration:** carga y aplica lo guardado.
- **Auto save:** si esta activado, guarda solo, un segundo despues de cada cambio.

### Window

- **Destroy interface:** cierra y destruye por completo el menu (pide confirmacion).

---

## Elementos que puedes crear

Todos se crean sobre una pestana: `Tab:CreateXxx({ ... })`. El orden de las lineas es el orden en pantalla. Cada uno devuelve un objeto que puedes guardar en una variable para modificarlo despues.

### Section

Linea con titulo que separa grupos de elementos.

```lua
Tab:CreateSection("Nombre de la seccion")
```

- Metodo: `:Set(nuevoNombre)`

### Label

Texto simple en una fila, con icono opcional a la izquierda.

```lua
local lbl = Tab:CreateLabel("Texto", iconoOpcional)
lbl:Set("Texto nuevo")
```

### Paragraph

Titulo en negrita con texto largo debajo. Se ajusta solo al ancho y crece en altura segun el contenido.

```lua
local p = Tab:CreateParagraph({ Title = "Titulo", Content = "Texto largo..." })
p:Set({ Title = "Nuevo titulo", Content = "Nuevo texto" })
```

### Button

Ejecuta una funcion al pulsarlo. Tiene una pequena animacion de parpadeo.

```lua
Tab:CreateButton({
	Name = "Mi boton",
	Callback = function()
		print("Pulsado")
	end,
})
```

- Metodo: `:Set(nuevoNombre)`

### Toggle

Interruptor de encendido/apagado. Verde y con halo luminoso cuando esta activo, gris cuando esta apagado. La bola se desliza con animacion.

```lua
local t = Tab:CreateToggle({
	Name = "Mi toggle",
	CurrentValue = false,
	Flag = "MiToggle",
	Callback = function(valor) -- true o false
		print(valor)
	end,
})
t:Set(true)
print(t.CurrentValue)
```

### Slider

Barra para elegir un numero dentro de un rango. Se arrastra con raton o dedo (mientras arrastras, la pagina no hace scroll). Muestra el valor actual a la derecha.

```lua
Tab:CreateSlider({
	Name = "Velocidad",
	Range = { 16, 200 },   -- minimo y maximo
	Increment = 1,         -- salto (admite decimales: 0.05)
	Suffix = "studs/s",    -- texto opcional tras el numero
	CurrentValue = 16,
	Flag = "MiSlider",
	Callback = function(valor)
		print(valor)
	end,
})
```

### Dropdown

Lista desplegable. Puede ser de una sola opcion o de **seleccion multiple**. Muestra a la derecha lo que esta elegido.

```lua
local d = Tab:CreateDropdown({
	Name = "Colores",
	Options = { "Rojo", "Verde", "Azul" },
	MultipleOptions = true,       -- false = una sola opcion
	CurrentOption = { "Rojo" },
	Flag = "MiDropdown",
	Callback = function(lista)    -- siempre llega una lista
		print(table.concat(lista, ", "))
	end,
})
d:Set({ "Verde" })
d:Refresh({ "A", "B", "C" })     -- cambia las opciones disponibles
```

- En modo de una opcion, al elegir se cierra solo.
- Las opciones elegidas se resaltan con el color de acento.

### Input

Caja de texto.

```lua
Tab:CreateInput({
	Name = "Nombre",
	PlaceholderText = "Escribe aqui...",
	CurrentValue = "",
	RemoveTextAfterFocusLost = false, -- true = se vacia al salir de la caja
	Flag = "MiInput",
	Callback = function(texto)
		print(texto)
	end,
})
```

- El `Callback` se ejecuta cuando la caja pierde el foco (al pulsar Enter o tocar fuera).

### Keybind

Boton para elegir una tecla. Al pulsarlo muestra `...` y espera tu tecla. **Escape cancela** el cambio.

```lua
Tab:CreateKeybind({
	Name = "Accion",
	CurrentKeybind = "F",
	HoldToInteract = false, -- true = el callback recibe true al pulsar y false al soltar
	Flag = "MiKeybind",
	Callback = function(valor)
		print(valor)
	end,
})
```

- Con `HoldToInteract = false`, el callback recibe el nombre de la tecla cada vez que se pulsa.
- Mientras se esta eligiendo tecla, el atajo de mostrar/ocultar el menu queda desactivado para evitar choques.

### ColorPicker

Bloque desplegable con un cuadro de saturacion/brillo y una barra de tono (hue). El color actual se ve en una muestra en la cabecera del bloque.

```lua
Tab:CreateColorPicker({
	Name = "Color",
	Color = Color3.fromRGB(135, 148, 255),
	Flag = "MiColor",
	Callback = function(color3)
		print(color3)
	end,
})
```

### CodeBlock

Marco de codigo de solo lectura con fuente monoespaciada, scroll en vertical y horizontal y un boton **Copy**. Ideal para mostrar ejemplos o scripts.

```lua
local cb = Tab:CreateCodeBlock({
	Title = "Mi script",
	Code = "print('hola')",
	Height = 240, -- altura del marco (opcional)
})
cb:Set("print('otro codigo')")
```

---

## Notificaciones

Tarjetas que aparecen en la esquina inferior derecha con una animacion de entrada. Se apilan si hay varias.

```lua
SpaceHub:Notify({
	Title = "Titulo",
	Content = "Mensaje opcional",
	Duration = 5,     -- segundos (por defecto 5)
	Image = 123456,   -- icono opcional (ID o nombre)
})
```

- Se cierran solas al acabar la duracion.
- **Tocar una notificacion la mantiene en pantalla** y evita que se cierre sola.
- Los errores dentro de un `Callback` se muestran automaticamente como notificacion "Callback error" y tambien en la consola, sin romper el menu.

---

## Sistema de keys

Pide una clave antes de abrir el menu. Se activa con `KeySystem = true`.

La ventana de keys muestra titulo, subtitulo, nota, una caja para escribir la clave y los botones:

- **Submit:** valida la clave (tambien con Enter). Si es incorrecta, muestra "Invalid key. Try again." y la ventana vibra.
- **Get key:** aparece solo si defines `KeyLink`. Copia el enlace al portapapeles.
- **Cancel:** cierra y detiene la carga del menu.

```lua
KeySystem = true,
KeySettings = {
	Title = "Space Hub",
	Subtitle = "Key System",
	Note = "Consigue tu key en el enlace.",
	FileName = "SpaceHubKey",
	SaveKey = true,            -- recuerda la key ya validada
	GrabKeyFromSite = false,   -- true = la key se descarga de una URL
	Key = { "SPACE-1234" },    -- una o varias keys validas
	KeyLink = "https://ejemplo.com/key",
},
```

Con `GrabKeyFromSite = true`, cada entrada de `Key` se trata como una URL y se compara con el texto que devuelve (sin espacios).

---

## Aviso de Discord

Ventana emergente que invita a unirse a tu servidor. Aparece una vez cargado el menu.

```lua
Discord = {
	Enabled = true,
	Invite = "tuInvite",       -- solo el codigo, sin discord.gg/
	RememberJoins = true,      -- no vuelve a mostrarse tras copiar la invitacion
	Title = "Join our Discord",
	Content = "Get updates, support and more.",
},
```

- **Copy invite:** copia `https://discord.gg/tuInvite` y avisa con una notificacion.
- **Not now:** cierra el aviso (volvera a salir la proxima vez).

---

## Temas

Hay 10 temas. Cada uno define color de fondo, color de lineas y color de acento. Al cambiar de tema todos los elementos se animan al nuevo color.

| Tema | Estilo |
|---|---|
| Space | Azul espacial (por defecto) |
| Titanium | Gris azulado claro |
| Graphite | Gris oscuro neutro |
| Cobalt | Azul intenso |
| Emerald | Verde azulado |
| Crimson | Rojo oscuro |
| Violet | Violeta |
| Steel | Acero azulado |
| Obsidian | Casi negro |
| Amber | Ambar calido |

```lua
Theme = "Emerald"              -- al crear la ventana
Window:SetTheme("Obsidian")    -- en cualquier momento
```

La lista completa tambien esta en `SpaceHub.Themes`.

---

## Guardado de configuracion y Flags

Un **Flag** es un nombre unico que le das a un elemento. Los elementos con Flag se guardan y se restauran.

Se guardan: Toggle, Slider, Dropdown, Input, Keybind y ColorPicker. Tambien se guardan el tema elegido, el tipo de menu y la tecla de mostrar/ocultar.

```lua
ConfigurationSaving = {
	Enabled = true,
	FolderName = "MiHub",   -- carpeta
	FileName = "config",    -- archivo (config.json)
},
```

```lua
SpaceHub:SaveConfiguration()  -- guardar ya
SpaceHub:LoadConfiguration()  -- cargar y aplicar
SpaceHub.AutoSave = false     -- desactivar guardado automatico
SpaceHub.Flags.MiToggle.CurrentValue -- leer un valor desde cualquier parte
```

Notas:

- Cada Flag debe ser distinto.
- Requiere un ejecutor con `writefile`, `readfile` e `isfile`. Si no los tiene, el guardado se omite sin dar error.
- El guardado automatico espera 1 segundo tras el ultimo cambio para no escribir demasiadas veces.

---

## Iconos

En `CreateTab`, `CreateLabel`, `Notify` y similares puedes usar:

- Un numero: `123456789`
- Un texto con ID: `"123456789"` o `"rbxassetid://123456789"`
- Una URL (`https://...`)
- Un nombre registrado: `"home"`

Para registrar nombres:

```lua
SpaceHub:SetIcons({
	home = "rbxassetid://1234567",
	settings = 7654321,
})
-- o desde una URL que devuelva una tabla:
SpaceHub:SetIcons("https://ejemplo.com/iconos.lua")
```

Si un icono no existe o no carga, simplemente no se muestra y el diseno se ajusta.

---

## Opciones de CreateWindow

| Opcion | Tipo | Descripcion |
|---|---|---|
| `Name` | string | Nombre del hub. Se muestra si el icono falla y en el boton "Show". |
| `Theme` | string | Tema inicial. |
| `ToggleKey` | KeyCode | Tecla para mostrar/ocultar. Por defecto `RightShift`. |
| `MobileButton` | bool/nil | `nil` = automatico (aparece en dispositivos tactiles). |
| `Width`, `Height` | number | Tamano inicial (dentro de los limites). |
| `TitleImage` | string | Imagen del icono de la cabecera. |
| `LoadingScreen` | bool | Mostrar la intro. |
| `LoadingDuration` | number | Segundos de la intro. |
| `LoadingLogo` | string | Logo de la intro. |
| `ConfigurationSaving` | tabla | `Enabled`, `FolderName`, `FileName`. |
| `Discord` | tabla | `Enabled`, `Invite`, `RememberJoins`, `Title`, `Content`. |
| `KeySystem` | bool | Activar el sistema de keys. |
| `KeySettings` | tabla | `Title`, `Subtitle`, `Note`, `FileName`, `SaveKey`, `GrabKeyFromSite`, `Key`, `KeyLink`. |

### Boton "Show" (movil)

En dispositivos tactiles, al ocultar el menu aparece un boton **Show** arriba al centro para volver a abrirlo, ya que no hay teclado. En PC se usa la tecla `ToggleKey`.

---

## API completa

### Libreria (`SpaceHub`)

| Funcion | Descripcion |
|---|---|
| `SpaceHub:CreateWindow(opciones)` | Crea el menu y devuelve la ventana. Si ya habia uno, lo destruye. |
| `SpaceHub:Notify(opciones)` | Muestra una notificacion. |
| `SpaceHub:SetIcons(tabla o url)` | Registra iconos por nombre. |
| `SpaceHub:SaveConfiguration()` | Guarda los valores. |
| `SpaceHub:LoadConfiguration()` | Carga los valores. |
| `SpaceHub:Destroy()` | Destruye toda la interfaz y desconecta eventos. |
| `SpaceHub.Flags` | Tabla con todos los elementos que tienen Flag. |
| `SpaceHub.Themes` | Lista de temas. |
| `SpaceHub.AutoSave` | Activa o desactiva el guardado automatico. |
| `SpaceHub.Version` | Version de la libreria. |

### Ventana (`Window`)

| Funcion | Descripcion |
|---|---|
| `Window:CreateTab(nombre, icono)` | Crea una pestana. |
| `Window:Notify(opciones)` | Igual que `SpaceHub:Notify`. |
| `Window:Minimize(true/false)` | Minimiza o restaura. |
| `Window:Hide()` / `Window:Show()` / `Window:Toggle()` | Oculta, muestra o alterna. |
| `Window:SetTheme(nombre)` | Cambia el tema. Devuelve `true` si existe. |
| `Window:SetToggleKey(KeyCode)` | Cambia la tecla de mostrar/ocultar. |
| `Window:SetMenuType("horizontal" / "vertical")` | Cambia la disposicion. |
| `Window:Destroy()` | Cierra el menu. |

### Pestana (`Tab`)

`CreateSection`, `CreateLabel`, `CreateParagraph`, `CreateButton`, `CreateToggle`, `CreateSlider`, `CreateDropdown`, `CreateInput`, `CreateKeybind`, `CreateColorPicker`, `CreateCodeBlock`.

---

## Ejemplo minimo

```lua
local SpaceHub = loadstring(game:HttpGet(
	"https://raw.githubusercontent.com/dream14696/Official-space-hub-1.1.0/refs/heads/main/Library%20space%20hub"
))()

local Window = SpaceHub:CreateWindow({
	Name = "Mi Hub",
	Theme = "Space",
	ConfigurationSaving = { Enabled = true, FolderName = "MiHub", FileName = "config" },
})

local Main = Window:CreateTab("Main")

Main:CreateSection("Basico")
Main:CreateButton({
	Name = "Saludar",
	Callback = function()
		SpaceHub:Notify({ Title = "Hola", Content = "Funciona!", Duration = 3 })
	end,
})
Main:CreateToggle({
	Name = "Activar algo",
	CurrentValue = false,
	Flag = "Algo",
	Callback = function(v) print(v) end,
})
Main:CreateSlider({
	Name = "Valor",
	Range = { 0, 100 },
	Increment = 1,
	CurrentValue = 50,
	Flag = "Valor",
	Callback = function(v) print(v) end,
})

SpaceHub:LoadConfiguration()
```

---

## Solucion de problemas

| Problema | Posible causa y solucion |
|---|---|
| El menu no aparece | Revisa que el enlace de la libreria sea correcto y que tu ejecutor soporte `HttpGet` y `loadstring`. Mira la consola (F9). |
| No se guarda nada | Tu ejecutor puede no tener `writefile`/`readfile`. Verifica que `ConfigurationSaving.Enabled` sea `true` y que cada elemento tenga `Flag`. |
| Un boton no hace nada | El error aparece en la consola y como notificacion "Callback error". |
| Desaparecio el menu | Pulsa la tecla de `ToggleKey` (por defecto `RightShift`) o el boton **Show** en movil. |
| Un icono no se ve | Es normal si el ID no existe o no carga: se usa un respaldo o simplemente no se muestra. |
| Copy dice "Unavailable" | Tu ejecutor no tiene `setclipboard`/`toclipboard`. |
| Dos Flags se pisan | Cada Flag debe ser unico en todo el menu. |

---

## Creditos y licencia

Hecho por **dream14696**
mas: anonimo
                                                    discord: https://discord.gg/3AdwRPp2H
