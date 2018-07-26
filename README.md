# Proyecto MANEJO USSD

### by Romell Dominguez
[![](snapshot/icono.png)](https://www.romellfudi.com/)

## Objetivo:
![](snapshot/device_recored.gif)


Para manejar la comunicación ussd, hay que tener presente que la interfaz depende del SO y del fabricante.

## USSD LIBRARY

Construir una clase que extienda de los servicios de accesibilidad:

![image](snapshot/G.png)

En ella capturara la información de la pantalla USSD con el SO la visualice, para ello existen 2 maneras:

* via código:

![image](snapshot/H.png)

* via xml, el cual deberas vincular en el manifest de tu aplicación:

```xml
<?xml version="1.0" encoding="utf-8"?>
<accessibility-service xmlns:android="http://schemas.android.com/apk/res/android"
    android:accessibilityEventTypes
        ="typeWindowStateChanged"
    android:packageNames="com.android.phone"
    android:accessibilityFeedbackType="feedbackGeneric"
    android:accessibilityFlags="flagDefault"
    android:canRetrieveWindowContent="true"
    android:description="@string/accessibility_service_description"
    android:notificationTimeout="0"/>
```


### Aplicación

Configuramos en el archivo build.gradle, la extensión para leer librerias *.aar (la cuál crearemos y exportaremos)

```gradle
allprojects { repositories { ...
        flatDir { dirs 'libs' } } }
```

Configuramos la dependencia de la libraria ussdlibrary mediante los prefijs {debugCompile: llamar a módulo de la libreria, releaseCompile: llamar al empaquetado *.aar}

```gradle
dependencies {
    ...

    debugCompile project(':ussdlibrary')
    releaseCompile(name: 'ussdlibrary-1.0.a', ext: 'aar')
}
```


Teniendo importada las dependencias, en el manifest de la aplicación se debe escribir el servicio con los permisos necesarios

![image](snapshot/J.png)

![image](snapshot/F.png)

### Uso de la línea voip

En esta sección dejo las líneas claves para realizar la conexión VOIP-USSD

```java
ussdPhoneNumber = ussdPhoneNumber.replace("#", uri);
Uri uriPhone = Uri.parse("tel:" + ussdPhoneNumber);
context.startActivity(new Intent(Intent.ACTION_CALL, uriPhone));
```

Una vez inicializado la llamada el servidor telcom comenzará a enviar las *famosas pantallas **ussd***

![image](snapshot/telcom.png)
