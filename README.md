## Sistema de recomendación

Para la realización de un sistema de recomendación que funcione dentro de la plataforma de búsqueda de empleo juvenil propuesta por la BBK en el marco del **Desafío de Tripulaciones del Bootcamp de Data Science BBK - The bridge** desarrollamos un algoritmo de recomendación personalizada basado en el contenido de datos explícitos. Este modelo se basa en la información importante contenida en el perfil del user (joven que busca empleo) y la información del ítem (proyecto propuesto por una empresa), para lo cual se deben extraer los campos o features necesarias de la Base de Datos que se usarán para hacer la comparación user-item.

El resultado final fue un algoritmo que para cada usuario genera una lista de proyectos recomendados, mejorando la posibilidad de ser empleado a través de la plataforma y su experiencia de usuario.

##### Ventajas del modelo
- No necesita datos de otro usuario.
- Puede recomendar a perfiles poco frecuentes.
- Puede recomendar ítems nuevos.
- Puede proveer una explicación de la recomendación listando los features que fueron relevantes.

##### Desventajas del modelo
- Requiere construir features significativas y técnicas de extracción más sofisticadas.
- El perfil del usuario tiene que poder ser representado como una función de esos features.
- No aprovecha interacciones de otros usuarios con los ítems.
- Se puede potenciar el efecto burbuja.

## Desarrollo del algoritmo

Para desarrollar dicho algoritmo empezamos extrayendo los campos contenidos en la Base de Datos que serán de interés para realizar la recomendación, es decir, que contienen atributos, características y contenido textual con el cual aplicar procesamiento de lenguaje natural (NLP). Una vez definidos los campos, establecimos como hacer las comparaciones y los filtros necesarios. Resultando en el siguiente esquema:

<meta http-equiv="Content-Type" content="text/html; charset=utf-8"><link type="text/css" rel="stylesheet" href="resources/sheet.css" ><div class="ritz grid-container" dir="ltr"><table class="waffle" cellspacing="0" cellpadding="0"><thead><tr><th class="row-header freezebar-origin-ltr"></th><th id="1167149761C0" style="width:438px;" class="column-headers-background">A</th></tr></thead><tbody><tr style="height: 20px"><th id="1167149761R0" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">1</div></th><td class="s0" dir="ltr">Filtro de &quot;proteccion&quot; (no recomendar proyectos sin vacantes)</td></tr><tr style="height: 20px"><th id="1167149761R1" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">2</div></th><td class="s1" dir="ltr">Numero de vacantes (proyecto)</td></tr><tr style="height: 20px"><th id="1167149761R2" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">3</div></th><td></td></tr><tr style="height: 20px"><th id="1167149761R3" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">4</div></th><td class="s2" dir="ltr">Filtros simples iniciales</td></tr><tr style="height: 20px"><th id="1167149761R4" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">5</div></th><td class="s3" dir="ltr">Area (estudiante) - Area (proyecto)</td></tr><tr style="height: 20px"><th id="1167149761R5" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">6</div></th><td class="s3" dir="ltr">Grado educacion (estudiante) - Grado minimo (proyecto)</td></tr><tr style="height: 20px"><th id="1167149761R6" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">7</div></th><td></td></tr><tr style="height: 20px"><th id="1167149761R7" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">8</div></th><td class="s4" dir="ltr">Recomendador &quot;directo&quot; (jaccard similarity)</td></tr><tr style="height: 20px"><th id="1167149761R8" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">9</div></th><td class="s5" dir="ltr">Habilidades (estudiante) - Habilidades (proyecto)</td></tr><tr style="height: 20px"><th id="1167149761R9" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">10</div></th><td></td></tr><tr style="height: 20px"><th id="1167149761R10" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">11</div></th><td class="s6 softmerge" dir="ltr"><div class="softmerge-inner" style="width:536px;left:-1px">Recomendador basado en contenido (NLP, TF-IDF, cosine similarity)</div></td></tr><tr style="height: 20px"><th id="1167149761R11" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">12</div></th><td class="s7" dir="ltr">Nombre grado (estudiante)</td></tr><tr style="height: 20px"><th id="1167149761R12" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">13</div></th><td class="s7" dir="ltr">Master o especializacion (estudiante)</td></tr><tr style="height: 20px"><th id="1167149761R13" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">14</div></th><td class="s7" dir="ltr">Cursos (estudiante)</td></tr><tr style="height: 20px"><th id="1167149761R14" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">15</div></th><td class="s7" dir="ltr">Descripcion (proyecto)</td></tr><tr style="height: 20px"><th id="1167149761R15" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">16</div></th><td class="s7" dir="ltr">Perfil (proyecto)</td></tr><tr style="height: 20px"><th id="1167149761R16" style="height: 20px;" class="row-headers-background"><div class="row-header-wrapper" style="line-height: 20px">17</div></th><td class="s7" dir="ltr">Habilidades (estudiante) - Habilidades (proyecto)</td></tr></tbody></table></div>

Como se observa en la tabla hay unos filtros iniciales que, si bien son sencillos de implementar, evitan hacer recomendaciones inútiles y mejoran la eficiencia del algoritmo al reducir los registros (items) a comparar con cada usuario que inicie sesión.

Debido a que nos basamos en datos sintéticos generados por nosotros para la creación y evaluación del modelo nos ahorramos gran parte del preprocesamiento de datos, salvo el necesario para las técnicas de procesamiento del lenguaje natural.

Para la comparación de las habilidades requeridas por el proyecto y las habilidades del estudiante usamos similitud Jaccard, la cual evalúa que tan similares son dos conjuntos de elementos, por lo cual es importante que las habilidades estén predefinidas por la plataforma para garantizar coincidencias entre ambos conjuntos.


Para hacer el análisis (NLP) del contenido en cada registro, unimos todo el texto contenido en los campos de interés del user (estudiante/joven) y del proyecto para aplicar TF-IDF y similitud coseno.

- TF-IDF: Es una medida estadística que evalúa la importancia de una palabra en un documento dentro de un grupo de documentos, ayuda a identificar palabras clave y a representar textos de manera más eficaz.

- Similitud Coseno: se utiliza para comparar la similitud entre dos textos representados como vectores de términos. En este caso, terminos resultados de la tokenización dada con TF-IDF.

Para realizar lo anterior de la mejor forma posible, usamos un preprocesamiento que consistió en:

- Llevar todo el texto a minúsculas.
- Quitar números y caracteres especiales.
- Aplicar lematización, que consiste en reducir las palabras a su forma base teniendo en cuenta contexto y estructura gramatical, lo que permite tratar palabras relacionadas de manera uniforme.

Una vez hechas las comparaciones y cálculos, se crea un score dado por la suma de la similitud coseno y la similitud Jaccard, que nos indicará esos proyectos que más coinciden con nuestro user y de esa forma retornar las k mejores recomendaciones.


## Mejoras
	
- Ubicacion geografica:	Usar la direccion de la empresa asociada al proyecto y la direccion del estudiante/joven.

- Disponibilidad en tiempo:	Usar la informacion de si el estudiante esta haciendo un proyecto actualmente, cuando termina este proyecto y cuando empieza el proximo a recomendar.

- Experiencia: Tener en cuenta si el estudiante esta graduado y si tiene experiencia o no y si el perfil del proyecto lo requiere.

- Usar OCR para procesar directamente el texto en los CV que suban los estudiantes en formato pdf.

- Hacer un algoritmo similar para que recomiende perfiles a las empresas añadiendo otros campos de interes como los logros del estudiante.


https://github.com/EfrainVisconti/Recommendation_system_with_NLP/assets/144735628/e0510968-6c87-4606-b903-67b749d4d3b0

