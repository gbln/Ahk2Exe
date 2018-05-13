# Ahk2Exe
###### Compilador no oficial para AutoHotkey v2 en español.
<p align="center">
  <img src="https://github.com/flipeador/Ahk2Exe/raw/master/preview.jpg" alt="Ahk2Exe For AHKv2"/>
</p>

##### Durante el procesado del script, se tienen en cuenta los siguientes factores, ordenados en forma descendente de importancia.

  1. `Mejorar el rendimiento`, por más insignificante que éste sea. Este es el objetivo más importante, debido a la lentitud extrema de los lenguajes interpretados como lo es AHK.
  2. Lograr `reducir al máximo el tamaño del código`, quitando espacios y utilizando equivalentes más cortos en expresiones.
  3. `Ofuscar el código` (hacerlo lo más confuso posible) sin perdidas de rendimiento ni aumento del tamaño del código en lo absoluto.

##### Debe tener en cuenta los siguientes puntos con respecto al compilador.

  - La compilación no garantiza la protección del código fuente.
  - La compilación no garantiza mejoras significativas de rendimiento.
  - AutoHotkey es un lenguaje interpretado, por lo que realmente no posee un compilador **real**, `Ahk2Exe` no realiza ningún pasaje de código AHK a código máquina, realmente no compila nada, sino que procesa el script para reducir su tamaño y facilita la adición de recursos en el archivo destino EXE. Al momento de "Compilar" un script, lo que en realidad se esta haciendo, es copiar el archivo `BIN` al destino especificado con la extensión `EXE`, y luego se le añade el script como un recurso en `RT_RCDATA`.





* * *


<br><br>


# Notas:
- Los archivos `Ahk2Exe.exe` y `Ahk2Exe64.exe` son totalmente independientes, no requieren de ningún otro archivo para su funcionamiento, aunque para poder compilar los scripts es necesario tener los archivos `BIN` en el mismo directorio que `Ahk2Exe.exe`.
- Para poder comprimir el archivo `EXE` resultante, es necesario tener `UPX` y/o `MPRESS` en el mismo directorio que `Ahk2Exe.exe`.
- La versión de 64-bit (`Ahk2Exe64.exe`) no soporta el efecto de agua en el logo `AHK`. La funcionalidad es exactamente la misma que en la de 32-bit.
- Por lo general, la compilación no mejora el rendimiento de un script.
- En el caso de una falla, `Ahk2Exe` tiene códigos de salida que indican el tipo de error que ocurrió.
- La codificación por defecto utilizada es `UTF-8 con BOM` (unicode), esto quiere decir que, si va a compilar un script sin `BOM`, el script compilado lo incluirá automáticamente, para asegurarse de que todos los caracteres (ej. `á`) se visualizen correctamente.





* * *


<br><br>


# Características:
- [x] Compilar Scripts (función principal).
- [x] La configuración se guarda en el registro, en donde se incluye las últimas opciones conocidas al momento de cerrar el compilador y además, almacena una lista con los 10 primeros archivos fuente e iconos en el control.
- [x] Detección de errores y registro de los mismos. Se incluye un control `ListView` dedicado explícitamente a guardar los registros de la compilación, para ayudarle a detectar los errores.
- [x] Detectar y remover comentarios en el script. Se incluye detección de comentarios en expresiones (actualmente no soportado por AHK), por ejemplo: `expr+1 /* comentario */ expr+1` --> `expr+1 expr+2`; comentarios en línea `;` y comentarios en bloque `/**/`.
- [x] Detectar y remover espacios innecesarios de cada línea.
- [x] Detectar y optimizar secciones de continuación.
- [x] Detectar y optimizar expresiones y cadenas.
- [x] Soporte para compilar por medio de la línea de comandos + códigos de errores.
- [x] Soporte para cambiar el icono principal.
- [x] Soporte para añadir cualquier tipo de iconos y cursores.
- [x] Soporte variado para añadir recursos en el ejecutable y crear nuevos tipos.
- [x] Soporte para establecer el lenguaje de los recursos añadidos
- [ ] Soporte para cambiar la información de la versión. **[PRÓXIMAMENTE TOTALMENTE NATIVO EN AHK]**
- [x] Soporte para cambiar el sub-sistema del ejecutable a modo consola.
- [x] Soporte completo para las directivas `#Include` y `#IncludeAgain`. Soporte para variables (debe encerrarlas entre `%`).
- [x] Soporte **parcial** para la función `FileInstall`. Evite los parentesis, expresiones (que no sean variables) y escriba la función en una línea completamente dedicada a ella. Soporte para variables.
- [x] Soporte para compresión del archivo compilado con `UPX` y `MPRESS`.





* * *


<br><br>


# Compilación por línea de comandos
Aquí se detalla la sintaxis para poder compilar por medio de la línea de comandos. Puede ver los códigos de salida más abajo.

El orden de los parámetros especificados importa, por ejemplo, si especifica primero `iconfile.ahk` sin una ruta absoluta, se tendra en cuenta el directorio del compilador; por el contrario, si especifica `infile.ahk` antes, se tendra en cuenta el directorio de `infile.ahk`.

- ##### Sintaxis
```Ahk2Exe.exe [/in] infile.ahk [/out outfile.exe] [/icon iconfile.ico] [/bin binfile.bin] [/upx] [/mpress] [/quiet]```

- ##### Descripción
| Parámetro  | Descripción | Directorio de trabajo |
| ---------- | ----------- | --------------------- |
| infile.ahk | Archivo fuente AHK (script) que se va a compilar (obligatorio). | Compilador |
| outfile.exe | Archivo destino EXE compilado. Si no se especifica, se establece en `infile.exe`. Si se especifica un directorio se establece en `\infile.exe`. Si no se especifica la extensión se añade automáticamente `.exe`; puede especificar cualquier extensión. Si el archivo ya existe, lo intenta sobreescribir. | infile.ahk o compilador |
| iconfile.ico | Icono principal del archivo compilado. Si no se especifica se mantiene el icono por defecto de AutoHotkey. El icono principal puede ser establecido por medio de la directiva del compilador `@Ahk2Exe-SetMainIcon`. Tenga en cuenta que establecer un icono elimina todos los iconos por defecto de AHK, incluyendo iconos de _pausa_ (pause) y _suspensión_ (suspend). | infile.ahk o compilador |
| binfile.bin | Archivo BIN AutoHotkey. Si no se especifica utiliza el último archivo BIN utilizado. En caso de no haber una configuración válida guardada se establece dependiendo de la arquitectura del compilador `Unicode %8*A_PtrSize%-bit`. Por ejemplo: `Unicode 64-bit` (la extensión es opcional) | Compilador |
| /upx o /mpress | Especifica el método de compresión del archivo EXE resultante. Los archivos `upx.exe` y `mpress.exe` deben estar en el directorio junto al compilador. | Compilador |
| /quiet o /q | Especifica que deben suprimirse todos los mensajes, diálogos y ventanas durante la compilación. Esta opción es útil si se aprovecha el código de salida, que le permite identificar el error ocurrido, si lo hubo. | - |





* * *


<br><br>


# Directivas específicas del compilador
El compilador acepta ciertas directivas que le permiten personalizar aún más el script compilado `.exe`.

##### ++Directivas que controlan el comportamiento del script++

  - **`;@Ahk2Exe-IgnoreBegin`**`[Opción]`

    Es posible eliminar secciones de código del script compilado al encerrarlas en las directivas `@Ahk2Exe-IgnoreBegin` y `@Ahk2Exe-IgnoreEnd` como si fueran comentarios multilinea en bloque `/**/`.
    ```autohotkey
    MsgBox "Este mensaje aparece tanto en el script compilado como en el no compilado"
    ;@Ahk2Exe-IgnoreBegin
    MsgBox "Este mensaje no aparece en el script compilado"
    ;@Ahk2Exe-IgnoreEnd
    MsgBox "Este mensaje aparece tanto en el script compilado como en el no compilado"
    ```

    ```autohotkey
    ;@Ahk2Exe-IgnoreBegin 32
    MsgBox "Este mensaje solo aparecerá en AHK de 64-Bit"
    ;@Ahk2Exe-IgnoreEnd

    ;@Ahk2Exe-IgnoreBegin 64
    MsgBox "Este mensaje solo aparecerá en AHK de 32-Bit"
    ;@Ahk2Exe-IgnoreEnd
    ```

  - **`/*@Ahk2Exe-Keep`**

    Lo contrario también es posible, es decir, marcar una sección de código para que solo se ejecute en el script compilado.

    ```autohotkey
    /*@Ahk2Exe-Keep
    MsgBox "Este mensaje aparece solo en el script compilado"
    */
    MsgBox "Este mensaje aparece tanto en el script compilado como en el no compilado"
    ```

<br><br>

##### ++Directivas que controlan los metadatos ejecutables++

  - **`;@Ahk2Exe-SetProp`**`Valor`    **(SIN SOPORTE AÚN)**

    Cambia una propiedad en la información de versión del ejecutable compilado. En la tabla siguiente se describen las propiedades disponibles y su descripción. Puede utilizar las propiedades descritas entre parentesis para evitar utilizar, por ejemplo, la propiedad `Name`, que cambia tanto el nombre del producto como el nombre interno.

    `Prop` Debe reemplazarse por el nombre de la propiedad a cambiar. Debe ser una de las descritas en la tabla de abajo.

    `Value` Es el valor a establecer a la propiedad. Puede ver la descripción de este valor en la tabla de abajo.

    | Propiedad | Descripción |
    | --------- | ----------- |
    | Name | Cambia el nombre del producto (`ProductName`) y el nombre interno (`InternalName`). |
    | Description | Cambia la descripción del archivo (`FileDescription`). |
    | Version | Cambia la versión del archivo (`FileVersion`) y la versión del producto (`ProductVersion`). Si esta propiedad no se modifica, se usa de forma predeterminada la versión de AutoHotkey utilizada para compilar el script. |
    | Copyright  | Cambia la información legal de copyright (derechos de autor). |
    | OrigFilename | Cambia la información del nombre de archivo original (`OriginalFileName`). |
    | CompanyName | Cambia el nombre de la compañía. |

  - **`;@Ahk2Exe-SetMainIcon`**`IcoFile`

    Establece el icono principal, esta directiva sobreescribe el icono especificado en la interfaz y línea de parámetros del compilador. Si utiliza esta directiva, antes de añadir el icono se eliminan todos los iconos por defecto de AHK, incluyendo los iconos de `Pausa` y `Suspensión`, quedando únicamente el icono por defecto especificado. El nombre del grupo en `RT_GROUP_ICON` es `159`, por lo que debe evitar añadir recursos iconos con este nombre mediante `AddResource`.

  - **`;@Ahk2Exe-PostExec`**`Comando`

    Especifica un comando que se ejecutará después de una compilación exitosa. La cadena especificada en `Comando` será ejecutada mediante la función incorporada en AHK `Run`.

  - **`;@Ahk2Exe-ConsoleApp`**

    Cambia el subsistema ejecutable al modo consola. Cuando se ejecute el archivo compilado EXE, se abrirá una ventana de consola. Esto modifica el valor de `IMAGE_OPTIONAL_HEADER.Subsystem` a `IMAGE_SUBSYSTEM_WINDOWS_CUI`.

  - **`;@Ahk2Exe-AddResource`**`FileName [, ResourceName]`

    Agrega un recurso al ejecutable compilado.

    `FileName` Es el nombre del archivo del recurso para agregar. El tipo del recurso (como un entero o una cadena) se puede especificar explícitamente anteponiendo un asterisco a él: `*tipo Filename` o `*"nombre tipo" Filename` si el nombre del tipo contiene espacios.

    `ResourceName` (opcional) Es el nombre que tendrá el recurso (puede ser una cadena o un número entero). Si se omite, el valor predeterminado es el nombre (sin ruta) del archivo, en mayúsculas (incluye la extensión).

    Aquí hay una lista de tipos de recursos estándar comunes y las extensiones que los desencadenan de forma predeterminada.

    | Tipo de recurso | Extensiones |
    | --------------- | ------------ |
    | 1 (RT_CURSOR) | .cur (cursores) |
    | 2 (RT_BITMAP) | .bmp; .dib |
    | 3 (RT_ICON) | .ico (iconos) |
    | 4 (RT_MENU) | - |
    | 5 (RT_DIALOG) | - |
    | 6 (RT_STRING) | - |
    | 9 (RT_ACCELERATORS) | - |
    | 10 (RT_RCDATA) | Cualquier otra extensión. Este es el recurso utilizado por la función `FileInstall`. |
    | 11 (RT_MESSAGETABLE) | - |
    | 23 (RT_HTML) | .htm; .html; .mht |
    | 24 (RT_MANIFEST) | .manifest |

    Además de los recursos especificados en la tabla de arriba, el compilador soporta los siguientes tipos de recursos que son detectados automáticamente por la extensión, o que puede especificarse de forma explícita: `*tipo`.

    | Tipo de recurso | Descripción |
    | --------------- | ----------- |
    | .PNG (RT_ICON) | Imágenes PNG |

  - **`;@Ahk2Exe-UseResourceLang`**`LangCode`

    Cambia el lenguaje de recursos utilizado por `@Ahk2Exe-AddResource`.

    `LangCode` Es el [código de idioma](https://msdn.microsoft.com/en-us/library/windows/desktop/dd318693%28v=vs.85%29.aspx). Tenga en cuenta que los números hexadecimales deben tener un prefijo `0x`.





* * *


<br><br>


# Códigos de salida (exitcodes)
Los códigos de salida indican el tipo de error que ocurrió durante la compilación. Esto le será útil cuando compila un script por medio de la línea de comandos.
  - **General**

     | Código de salida | Constante | Descripción |
     | --- | --- | --- |
     | 0x00 | ERROR_SUCCESS | Todas las operaciones se han realizado con éxito |
     | 0x01 | UNKNOWN_ERROR | Error desconocido |
     | 0x02 | ERROR_NOT_SUPPORTED | No soportado |
     | 0x03 | ERROR_INVALID_PARAMETER | Los parámetros pasados son inválidos |

  - **Apertura de archivos**

     | Código de salida | Constante | Descripción |
     | ---------------- | --------- | ----------- |
     | 0x10 | ERROR_SOURCE_NO_SPECIFIED | El archivo fuente no se ha especificado |
     | 0x11 | ERROR_SOURCE_NOT_FOUND | El archivo fuente no existe |
     | 0x12 | ERROR_CANNOT_OPEN_SCRIPT | No se ha podido abrir el archivo fuente script (incluyendo includes) para lectura |
     | 0x13 | ERROR_BIN_FILE_NOT_FOUND | El archivo BIN no existe |
     | 0x14 | ERROR_BIN_FILE_CANNOT_OPEN | No se ha podido abrir el archio BIN para lectura |
     | 0x15 | ERROR_MAIN_ICON_NOT_FOUND | El icono principal no existe |
     | 0x16 | ERROR_MAIN_ICON_CANNOT_OPEN | No se ha podido abrir el icono principal para lectura |
     | 0x17 | ERROR_INVALID_MAIN_ICON | El icono principal es inválido |
     | 0x18 | ERROR_INCLUDE_FILE_NOT_FOUND | El archivo a incluir no existe |
     | 0x19 | ERROR_INCLUDE_DIR_NOT_FOUND | El directorio a incluir no existe |
     | 0x20 | ERROR_FILEINSTALL_NOT_FOUND | El archivo a incluir especificado en FileInstall no existe |
     | 0x21 | ERROR_RESOURCE_FILE_NOT_FOUND | El archivo de recurso a incluir no existe |

  - **Escritura de archivos**

     | Código de salida | Constante | Descripción |
     | ---------------- | --------- | ----------- |
     | 0x30 | ERROR_CANNOT_COPY_BIN_FILE | No se ha podido copiar el archivo BIN al destino |
     | 0x31 | ERROR_CANNOT_OPEN_EXE_FILE | no se ha podido abrir el archivo destino EXE para escritura |

  - **Sintaxis**

     | Código de salida | Constante | Descripción |
     | ---------------- | --------- | ----------- |
     | 0x50 | ERROR_INVALID_SYNTAX_DIRECTIVE | La sintaxis de la directiva es inválida |
     | 0x51 | ERROR_UNKNOWN_DIRECTIVE_COMMAND | Comando de directiva desconocido |
     | 0x52 | ERROR_FILEINSTALL_INVALID_SYNTAX | La sintaxis de FileInstall es inválida |





* * *


<br><br>


# Pensamientos para futuras actualizaciones
- Mejorar el procesado del Script para reducir al máximo el tamaño del archivo compilado y, si es posible, mejorar el rendimiento.
- Añadir soporte para modificar la información de la versión.
- Mejorar el soporte para incluir recursos.
- Mejorar soporte de la función `FileInstall`.
- Implementar más opciones en la interfaz.
