### FrameWork RT1 ###

![#](/Documentation/img/0.png)

**Resumen**

Framework para procesos sencillos que se componen de pocas acciones secuenciales.
Para el productor o dispatcher del modelo tipico Productor-Consumidor.
Para procesos sencillos de tipo no transactional.

##### Argumentos
    in_ConfigPath - Default value: "Data\Config.json"


##### Guía básica de uso 
1) Descarga directamente la última release [RPATechnologies_RT1_Template.zip]
2) Descomprime el .zip en tu directorio Documents\UiPath\
3) Renombra el proyecto a NombreCliente_AliasProcesoGeneral_RT1_0X_NombreDelProceso (Por ejemplo NIMOGORDILLO_CVN_RT1_01_DescargarExpedientes)
4) Incluye las variables y assets necesarios para el proceso en el archivo de configuración "Data\Config.json" respetand la estructura del ejemplo.
5) Desarrolla en un xaml independiente "Process.xaml" las acciones necesarias para la ejecución del proceso.(Usa el modelo Sequence, Flowchart o State machineque mejor se ajuste a la necesidad.)
6) Invoca el xaml "Process.xaml" dentro del Process State.


**1. Configuration State**

![#](/Documentation/img/1.png)

##### ConfigFramework
Variables de configuración del Framework
-ProjectName
-InitTime
-Resolution
-UserName
-Machine
-ResultStatus
-ResultMessage

##### ConfigProcess
Variables de configuración necesarias apra la ejecución. <br>
El fichero Config contiene la información de Settings, Assets y Credentials. <br>
Debe respetarse la estructura de JArray del ejemplo de archivo "Config.json" para que tanto las variables como los assets del orquestador puedan ser obtenidos al inicio de la ejecución.<br>

**¡IMPORTANTE!** <br>
Si el proceso requiere del uso de assets, y/o Credentials, NO modificar la variable **OrchestratorFolder** del archivo "config.json"
**Esta variable actua de discriminador para recoger o no los assets y credenciales**

Si no se modifica:
![#](/Documentation/img/A.png)
<br>

Si se elimina o modifica el "Name":
![#](/Documentation/img/B.png)

Se dejara el warning advirtiendo de que el config.json contiene array de Assets y/o credenciales pero no se ha indicado la carpeta. Pero la ejecución continuará con los settings.



**2. Process State**

![#](/Documentation/img/3.png)

En este state será donde invocaremos el "Process.xaml" que ha sido desarrollado con las acciones que debe realizar el proceso. <br>

En caso de excepción de tipo System o Business dentro de este xaml, la excepción debe ser relanzada con un throw o rethrow para que pueda ser capturada por el framework en este Process State.
Una vez levantada la excepción desde el "Process.xaml", su gestión queda contemplada por el framework y no requiere ninguna modificación. 

![#](/Documentation/img/4.png)



**3. Final State**

![#](/Documentation/img/5.png)

Tanto en caso de ejecución correcta como de excepción, el proceso pasará por esta fase antes de completar la ejecución. <br>

El objetivo es liberar el entorno de ejecución mediante el cierre de aplicaciones independientemente del desenlace. <br>

Si fuera necesario realizar alguna acción en caso de error, por ejemplo, enviar un correo de notificación, dicha acción será incluida en este punto del framework. <br>

![#](/Documentation/img/7.png)

En caso de error durante esta acción adicional, la excepción será también capturada y el mensaje de error concatenado al mensaje previo de error del process. 
Una vez limpiado el entorno y llevado a cabo las posibles acciones que fueran necesarias, la excepción será levantada para setear el job como Failed en el orquestador de cara a la gestión de su mantenimiento.




