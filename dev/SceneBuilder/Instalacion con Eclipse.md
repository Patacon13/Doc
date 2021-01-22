# Scene Builder

Para construir aplicaciones en JavaFX se puede usar Scene Builder de Gluon como un generador de archivos FXML.
Página oficial para descarga:

```
https://gluonhq.com/products/scene-builder/
```

# Eclipse

Para la edición de código se puede usar el IDE Eclipse de IBM. Este se puede descargar con un comando desde Ubuntu

```
sudo snap install --classic eclipse
```

# JavaFX

Conjunto de utilidades para la creación de aplicaciones de escritorio en el lenguaje Java. Desde Ubuntu se puede instalar con el comando:

```
sudo apt-get install openjfx
```

## Instalar JavaFX en Eclipse

Una vez descargado JavaFX, se debe instalar en eclipse:

```
Eclipse -> Window -> Preferences -> Java -> Build Path -> User Libraries -> New
```

Como nombre se inserta "JavaFX11", luego, presionando "Add external JARS" se agregarán los archivos .jar descargados de JavaFX (En ubuntu están ubicados en /usr/share/openjfx/lib).
 
![JavaFX instalado en Eclipse](images/JavaFX-Eclipse.jpg)

## Instalar E(fx)lipse a Eclipse

E(fx)lipse es un útil plug-in para el IDE Eclipse. Permite manejar con mayor facilidad los proyectos relacionados a JavaFX. Se puede instalar recurriendo a

```
Eclipse -> Help -> Eclipse MarketPlace -> find [Buscar E(fx)clipse y presionar el botón Install].
```

Finalmente, indicar a E(fx)clipse donde está instalado Scene Builder

```
Eclipse -> Window -> Preferences -> JavaFX
```

En ubuntu las configuraciones son:

SceneBuilder executable: /opt/scenebuilder/bin/SceneBuilder

JavaFX11 SDK: /usr/share/openjfx/lib

## Hello World

Una forma simple de iniciar un proyecto JavaFX es hacerlo desde Eclipse. Para esto, se genera un nuevo proyecto de tipo JavaFX.

1) Generar proyecto

```
Eclipse -> File -> New -> Other... -> JavaFX -> JavaFX Project
```


2) Insertar un nombre y crear el proyecto.


3) Agregar librería de JavaFX

```
Project -> Properties -> Libraries -> Classpath -> Add Library... -> User Library
```

4) Agregar a la VM la configuración de JavaFX

```
Eclipse -> Run -> Run Configurations -> Dependencies -> Override Dependencies 
```

Agregar: --module-path /path/to/javafx-sdk-15.0.1/lib --add-modules javafx.controls,javafx.fxml

Reemplazar reemplazar /path/to/javafx-sdk-15.0.1/lib por el path a javafx

5) Crear un archivo .FXML

```
Eclipse -> [En el paquete de la clase principal] New -> Other... -> JavaFX -> New JFXL Document 
```

6) Cambiar método Start por

```Java
@Override
public void start(Stage primaryStage) {
	try {
		FXMLLoader loader = new FXMLLoader();
		Parent root = FXMLLoader.load(getClass().getResource("MainFXML.fxml"));
		primaryStage.setScene(new Scene(root));
		primaryStage.show();
	} catch(Exception e) {
		e.printStackTrace();
	}
}
```

7) Editar desde SceneBuilder

```
[Click derecho en archivo .JFXL]->Open with Scene Builder

```
