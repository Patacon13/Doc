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

## Pipes

Inicialización:

```c++
int fd[2];
pipe(fd);
```

## Funciones útiles

| Función | Descripción |
|:-------:|:------------:
|fork()|Crea un nuevo proceso hijo. Retorna el ID del proceso.|
|wait(&wstatus)|Detiene el funcionamiento del proceso hasta que el hijo termine. Devuelve el estado del hijo en la variable pasada por referencia.|
|write(pipe[1], &var, sizeof(var))|Pone en la tubería, el valor de la variable|
|read(pipe[0], &var, sizeof(var))|Pone en la variable, el valor de la tubería|
|execlp("",...,"")|Ejecuta un nuevo proceso. Retorna -1 si no lo encontró|

## Macros útiles

| Macro | Descripción |
|:-----:|:-----------:|
|WIFEXITED|Verifica si el hijo terminó bien. Retorna true en ese caso. False en caso contrario|
|WEXITSTATUS|Verifica si la ejecución fue exitosa. 0 en ese caso. Distinto de 0 en caso contrario|

# Manejo de archivos

## Funciones útiles

| Función | Descripción |
|:-------:|:-----------:|
|open("",flags)|Abre el archivo, se le pueden indicar tantas flags como sean necesarias|

## Flags útiles

| Flag | Descripción |
|:----:|:-----------:|
|O_WRONLY|Solo escritura|
|O_CREATE|Crea el archivo|

# Comunicación entre procesos

Utilizaremos una cola común de mensajes para comunicarnos entre procesos.

## Funciones

 Función | Descripción |
|:-------:|:-----------:|
|ftock)("","")|Genera una nueva clave IPC|
|msgget(key, msgflg)|Devuelve el ID de la cola o -1. Crea la cola de ser necesario|
|msgsend(idCola, *ptrAStructDatos, tamanoDatos, msgflg')|Agrega datos a la cola|
|msgrcv(idCola,*EstructuraParaRecibir, tamanoDatos, tipoMensaje'', msgflg');
|msgctl(idCola, int cmd''', *buf'''')|Sirve para hacer cosas con la cola. Generalmente la eliminamos|

'msgflg por ahora en 0
''en 0 recibe cualquier cosa, positivo recibe el tipo que le pongas, negativo recibe el primero menor o igual al valor absoluto de lo que le pongas.
'''Para borrar, IPC_RMID.
''''Se deja en NULL.

## Estructura de mensajes a enviar

```c
struct msgbuf {
  long mtype; //un numero
  char mtext[1]; //dato a ingresar en la cola
  //Se pueden agregar más cosas
};
```

