## Analisis Lexico

#### Digrafos y Trigrafos

Son símbolos de 2 o 3 carácteres (respectivamente), que se usan como unidades de traducción para otros símbolos del lenguaje. El objetivo es poder "portarlo" y usarlo en teclados de otros lenguajes.
    
Ejemplos:

    Digrafo:  :>   ==  ]
    Trigrafo: ??=  ==  #

Los digrafos son reemplazados durante la **tokenización**, en el proceso de **análisis léxico**,
parte de la compilación, mientras que los trigrafos son reemplazados por el **preprocesador**.

#### Tabla de lexemas y tokens

    1: int printf(const char*, ...);
    2: int main(void){
    3:     int _[] = {-!.0,};
    4:     printf("%d%d", sizeof _ - sizeof _[0], sizeof(char) + 0[_]);
    5: }

Nro linea | Lexema | Token
--- | --- | ---
1 | int | palabraReservada
1 | printf | identificador
1 | () | caracterPuntuacion
1 | const | palabraReservada
1 | char | palabraReservada
1 | * | operador
1 | , | caracterPuntuacion
1 | ... | caracterPuntuacion
1 | ; | caracterPuntuacion
2 | int | palabraReservada
2 | main | identificador
2 | () | caracterPuntuacion
2 | void | palabraReservada
2 | { | caracterPuntuacion
3 | int | palabraReservada
3 | _ | identificador
3 | [] | caracterPuntuacion
3 | = | operador
3 | {} | caracterPuntuacion
3 | - | operador
3 | ! | operador
3 | .0 | constante
3 | ; | caracterPuntuacion
4 | printf | identificador
4 | () | operador
4 | "%d%d" | literalCadena
4 | , | operador
4 | sizeof | palabraReservada
4 | _ | identificador
4 | - | operador
4 | sizeof | palabraReservada
4 | _ | identificador
4 | [] | operador
4 | 0 | constante
4 | , | operador
4 | sizeof | palabraReservada
4 | () | operador
4 | char | palabraReservada
4 | + | operador
4 | 0 | constante
4 | [] | opereador
4 | _ | identificador

Todos los carácteres se pudieron traducir en lexemas de forma correcta y *tokenizar*.

Ya que no se encuentran errores, la *UT* es léxiamente correcta.

## Análisis Sintáctico

#### Árbol de derivación

    unidad-de-traduccion

    unidad-de-traduccion declaracion-externa

    unidad-de-traduccion declaracion-externa declaracion-externa

    declaracion-externa declaracion-externa declaracion-externa

**Analizo la primera declaracion-externa**

    declaracion-externa

    declaracion

    especificadores-de-declaracion lista-declaradores-init;

    especificador-de-tipo declarador-init;

    int declarador;

    int declarador-directo;

    int declarador-directo (lista-tipos-de-parametros);

    int identificador(lista-de-parametros, ...);

    int printf(declaracion-parametro, ...);

    int printf(especificaciones-de-decalaracion declarador, ...);

    int printf(calificador-de-tipo especificadores-de-declaracion declarador, ...);

    int printf(const especificador-de-tipo declarador, ...);

    int printf(const char apuntador declarador-directo, ...);

    int printf(const char * identificador, ...);

    int printf(const char * , ...);

Analizo la segunda declaracion-externa

    declaracion-externa

    definicion-de-funcion

    especificaciones-de-decalaracion declarador

    especificador-de-tipo declarador-directo

    int declarador-directo(lista-tipos-de-parametros)

    int identificador(lista-de-parametros)

    int main(declaracion-parametro)

    int main(especificadores-de-declaracion)

    int main(especificador-de-tipo)

    int main(void)

Analizo la tercera declaracion-externa

    declaracion-externa

    definicion-de-funcion

    proposicion-compuesta

    { lista-declaracion lista-de-proposiciones }

    { declaracion lista-de-proposiciones }

    { especificadores-de-declaracion lista-declaradores-init; lista-de-proposiciones }

    { especificador-de-tipo declarador-init; lista-de-proposiciones }

    { int declarador = inicializador; lista-de-proposiciones }

    { int declarador-directo = { lista-de-inicializadores, }; lista-de-proposiciones }

    { int identificador = { inicializador, }; lista-de-proposiciones }

    { int _ = { expresion-asignacion, }; lista-de-proposiciones }

    { int _ = { expresion-unaria, }; lista-de-proposiciones } (se resumio de expresion-asignacion a expresion-unaria)

    { int _ = { operador-unario expresion-cast, }; lista-de-proposiciones }

    { int _ = { - expresion-unaria, }; lista-de-proposiciones }

    { int _ = { - operador-unario expresion-cast, }; lista-de-proposiciones }

    { int _ = { - ! constante-flotante, }; lista-de-proposiciones } (se resumio de expresion-cast a constante-flotante)

    { int _ = { - ! .0, }; lista-de-proposiciones }

    { int _ = { - ! .0, }; proposicion }

    { int _ = { - ! .0, }; proposicion-expresion }

    { int _ = { - ! .0, }; expresion }

    { int _ = { - ! .0, }; expresion-de-asignacion } 

    { int _ = { - ! .0, }; expresion-posfija } (se resumion de expresion-de-asignacion a expresion-posfija)

    { int _ = { - ! .0, }; expresion-posfija(lista-de-expresiones-argumento) }

    { int _ = { - ! .0, }; expresion-primaria(lista-de-expresiones-argumento, expresion-de-asignacion) }

    { int _ = { - ! .0, }; identificador(lista-de-expresiones-argumento, expresion-de-asignacion, expresion-de-asignacion) }

    { int _ = { - ! .0, }; printf(expresion-de-asignacion, expresion-de-asignacion, expresion-de-asignacion) }

    { int _ = { - ! .0, }; printf(constante-de-caracter, expresion-aditiva, expresion-aditiva) }

    { int _ = { - ! .0, }; printf("%d%d", expresion-aditiva - expresion-aditiva, eexpresion-aditiva + eexpresion-aditiva) }

    { int _ = { - ! .0, }; printf("%d%d", expresion-unaria - expresion-unaria, expresion-unaria + expresion-unaria) }

    { int _ = { - ! .0, }; printf("%d%d", sizeof expresion-unaria - sizeof expresion-unaria, sizeof(nombre-de-tipo) + expresion-posfija) }

    { int _ = { - ! .0, }; printf("%d%d", sizeof identificador - sizeof expresion-posfija, sizeof(char) + expresion-posfija[expresion]) }

    { int _ = { - ! .0, }; printf("%d%d", sizeof _ - sizeof expresion-posfija, sizeof(char) + constante-de-caracter[identificador]) }

    { int _ = { - ! .0, }; printf("%d%d", sizeof _ - sizeof expresion-posfija[expresion], sizeof(char) + 0[_]) }

    { int _ = { - ! .0, }; printf("%d%d", sizeof _ - sizeof identificador[constante-de-caracter], sizeof(char) + 0[_]) } (nota: zzz significa underscore)

    { int _ = { - ! .0, }; printf("%d%d", sizeof _ - sizeof _[0], sizeof(char) + 0[_]) }

Uniendo los 3 analisis...

    int printf(const char * , ...);
    int main(void)
    { int _ = { - ! .0, }; printf("%d%d", sizeof _ - sizeof _[0], sizeof(char) + 0[_]) }

La *UT* es sintácticamente correcta.

## Análisis Semántico

La *UT*, siendo léxicamente correcta, no viola ninguna restricción semántica por lo que es semánticamente correcto.

#### Lo que hace el programa

    1: int printf(const char*, ...);
    2: int main(void){
    3:     int _[] = {-!.0,};
    4:     printf("%d%d", sizeof _ - sizeof _[0], sizeof(char) + 0[_]);
    5: }
    
El progama primero declara el prototipo de la función `printf`. Dentro de la función `main` se declara e inicializa un array de int con el valor -1 como único elemento. Por último, se invoca a la función printf para mostrar por pantalla 2 enteros: la resta entre el tamaño en bytes del array declarado y el tamaño del primer elemento del mismo; y la suma entre el tamaño definido por el estandar al tipo de dato char y el elemento indicado  con el incide 0 del array.

El tamaño del array es 4, el tamaño del primer elemento es 4 (por ser int), el tamaño de char es 1 (definido en el estandar) y el elemento indicando con el indice 0 del array es -1.

Por lo tanto, la salida del programa es `00`

###### declaraciones

En la linea 1 se declara la función `printf`, la misma no se define.

En la línea 2 se define la función `main`

En la línea 3 se declara la variable `_` como un array de `int`

###### expresiones

En la línea 3 se ve la expresión `-!.0,` que se evalúa a la cosntante `-1`

En la línea 4 se tiene la expresión primaria literalCadena `"%d%d"`

En la línea 4 se tiene la expresión aditiva `sizeof _ - sizeof _[0]` compuesta por 2 expresiones: `sizeof _` y `sizeof _[0]` unidas por el operador `-`

En la línea 4 se tiene la expresión aditiva `sizeof(char) + 0[_]` compuesta por 2 expresiones: `sizeof(char)` y `0[_]` unidas por el operador `+`

###### sentencias

Las líneas 3 y 4 conforman la sentencia compuesta (bloque) correspondiente a la función `main`

En la línea 3 se ve la sentencia de asignación `int _[] = {-!.0,};`

La línea 4 se tiene la sentencia de invocación de `printf();`
