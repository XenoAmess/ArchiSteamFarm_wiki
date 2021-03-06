# Depreciación

A partir de ASF V3.1.2.2, seguiremos una política de depreciación consistente para hacer tanto el desarrollo como el uso mucho más consistentes.

* * *

## ¿Qué es la depreciación?

Depreciación es el proceso de hacer cambios pequeños o grandes que vuelven obsoletos las opciones, argumentos, funcionalidades o casos de uso previos. La depreciación usualmente significa que una cosa dada fue reescrita en otra (similar) forma, y debes asegurarte de manera oportuna de cambiar a la nueva versión. En este caso, simplemente es mover una funcionalidad dada a un lugar más apropiado.

ASF cambia rápidamente y siempre busca mejorar. Esto lamentablemente significa que podemos cambiar o mover alguna funcionalidad existente a otro segmento del programa para que se beneficie de nuevas características, compatibilidad o estabilidad. Gracias a eso no necesitamos apegarnos a decisiones de desarrollo obsoletas o simplemente equivocadas que hayamos tomado hace años. Siempre intentamos proporcionar un reemplazo razonable que se ajuste al uso de funcionalidades disponibles previamente, por lo cual la depreciación es mayormente inofensiva y requiere pequeños arreglos a lo que se usaba con anterioridad.

* * *

## Etapas de la depreciación

ASF seguirá 2 etapas de deprecación, haciendo que la transición sea mucho más fácil y menos problemática.

### Estapa 1

La etapa 1 ocurre cuando una característica dada se deprecia, con disponibilidad inmediata de otra solución (o ninguna si no hay planes para reintroducirla).

Durante esta etapa, ASF mostrará la advertencia respectiva cuando se usa una función depreciada. Mientras sea posible, ASF intentará imitar el comportamiento antiguo y seguirá siendo compatible con él. ASF seguirá estando en la etapa 1 en cuanto a esa funcionalidad al menos hasta la siguiente versión estable. Este es el momento en que, con la esperanza de no romper el comportamiento, puedes hacer el cambio correspondiente en todas tus herramientas y patrones para satisfacer el nuevo comportamiento. Puedes confirmar que hiciste todos los cambios apropiados al ya no ver la advertencia de depreciación.

### Etapa 2

La etapa 2 está programada después de que la etapa 1 se produzca y sea publicada en una versión estable. Esta etapa introduce la completa eliminación de la característica obsoleta, lo que significa que ASF ni siquiera reconocerá que estás intentando usar una característica obsoleta, mucho menos respetarla, ya que simplemente no existe en el código actual. ASF ya no mostrará ninguna advertencia, porque ya no reconoce lo que estás intentando hacer.

* * *

## Sumario

Tienes más o menos un **mes completo** para hacer el cambio correspondiente, lo que debe ser más que suficiente incluso si eres un usuario casual. Después de ese período, ASF ya no garantiza que los ajustes antiguos tenga ningún efecto (etapa 2), haciendo que ciertas características dejen de funcionar sin que te des cuenta. Si estás iniciando ASF después de más un mes de inactividad, se te recomienda **[empezar de cero](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Setting-up-es-es)** nuevamente, o leer todos los registros de cambio que te hayas perdido y manualmente adaptar tu uso al actual.

En la mayoría de los casos, ignorar la advertencia de depreciación no hará que la funcionalidad general de ASF se vuelva inutilizable, sino más bien regresará al comportamiento por defecto (lo que puede o no coincidir con tus preferencias personales).

* * *

## Ejemplo

Previo a la V3.1.2.2 movimos el **[argumento de la línea de comandos](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Command-line-arguments-es-es)** `--server` a la **[propiedad de configuración global](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration-es-es#configuración-global)** `IPC`.

### Etapa 1

La etapa 1 ocurrió en la versión V3.1.2.2 donde añadimos la advertencia correspondiente al uso de `--server`. El argumento `--server` ahora obsoleto era mapeado automáticamente a la propiedad de configuración global `IPC: true`, actuando exactamente igual que el antiguo `--server`. Esto permitió a todos hacer el cambio correspondiente antes de que ASF dejara de aceptar el argumento antiguo.

### Etapa 2

La etapa 2 ocurrió en la versión V3.1.3.0, justo después de la versión V3.1.2.9 estable con la etapa 1 explicada anteriormente. La etapa 2 hizo que ASF dejara de reconocer por completo el argumento `--server`, tratándolo como cualquier otro argumento no válido, que ya no tiene ningún efecto en el programa. Para las personas que aún no cambiaron su uso de `--server` a `IPC: true`, hizo que la IPC dejara de funcionar completamente, porque ASF ya no hacía el mapeo apropiado.