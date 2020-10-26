# Compilar archivos

## Compilador gcc
Se puede utilizar gcc para compilar -o indica el nombre de la salida (output). Es opcional, en caso de no usarlo, saldrá un archivo ".out".

```
gcc nombre.c -o salida
```

## Ejecución

```
salida
```

# Argumentos

## Introducción

Los argumentos se reciben dentro de un vector de caracteres. Existen dos variables que nos pueden ser útiles:

|Palabra reservada | Tipo | Descripción |
|:------------------:|:------:|-------------|
| `argc` | int | contador de cantidad de argumentos |
| `argv` | array | arreglo de caracteres con los argumentos |

## Funciones útiles

Al trabajar con cadenas de caracteres, puede ser util tener en cuenta algunas funciones de los strings de C.

### Funciones generales - Librería cstring

| Función | Descripcion |
|:-------:|:-----------:|
|strlen(str)|Obtiene el tamaño del string|
|strlwr(str)|Convierte a minúsculas|
|strupr(str)|Convierte a mayúsculas|
|strcat(destStr, srcStr)|Concatena strings|
|strcpy(destStr, srcStr)|Copia un string a otro|
|strncpy(destStr, srcStr, n)|Copia los primeros n caracteres|
|strcmp(str1,str2)|Compara dos strings|
|strncmp(str1,str2)|Compara los primeros n caracteres de dos strings|
|stricmp(str1,str2)|Compara dos strings sin fijarse mayúsculas-minúsculas|
|strnicmp(str1,str2)|Compara los primeros n caracteres de dos strings sin fijarse mayúsculas-minúsculas|
|strchr(str,caracter)|Busca la primera ocurrencia del caracter en el string|
|strrchr(str,caracter)|Busca la última ocurrencia del caracter en el string|
|strstr(str1,str2)|Busca la primera ocurrencia de str2 en str1|
|strset(str,caracter)|Reemplaza el valor de cada posición de la cadena por el caracter|
|strnset(str,caracter,n)|Reemplaza los valores de las primeras n posiciones de la cadena por el caracter|
|strrev(str)|Da vuelta el string|

### Funciones de conversion - Librería cstdlib

| Función | Descripción |
|:-------:|:-----------:|
|atof(str)|Convierte un string a un double|
|atoi(str)|Convierte un string a un int|
|atol(str)|Convierte un string a un long|
|atoll(str)|Convierte un string a un long long|
|stroul(str)|Convierte un string a unsigned long|
|stroull(str)|Convierte un string a unsigned long long|

# Procesos

## Fork
Se pueden crear procesos hijos mediante la función:

| Función | Descripción |
|:-------:|:------------:
|fork()|Crea un nuevo proceso hijo. Retorna el ID del proceso.|
