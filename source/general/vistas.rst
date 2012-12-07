Las Vistas
====   

.. contents:: Es buena práctica de desarrollo que las vistas contengan una cantidad mínima de código en PHP para que sea suficientemente entendible para un diseñador Web y además, para dejar a las vistas solo las tareas de visualizar los resultados generados por los controladores y presentar las capturas de datos para usuarios.


Describiendo el funcionamiento
----

El manejador de vistas implementa el patrón de diseño de vista en dos pasos, el cual consiste en dividir el proceso de mostrar una vista en dos partes: la primera parte es utilizar una vista o ``view`` asociada a una acción del controlador para convertir los datos que vienen del modelo en lógica de presentación sin especificar ningún formato específico y la segunda es establecer el formato de presentación a través de una plantilla o template.

Así mismo tanto las vistas de acción como las plantillas pueden utilizar vistas parciales o partials. Estas vistas parciales son fragmentos de vistas que son compartidas por distintas vistas, de manera que constituyen lógica de presentación reutilizable en la aplicación. Ejemplos: menús, cabeceras, pies de página, entre otros.

KumbiaPHP favoreciendo siempre los convenios asume los siguientes respecto a las vistas:

- Todos los archivos de vistas deben tener la extensión ``.phtml``.
- Cada controlador o módulo tiene un directorio de vistas asociado cuyo nombre coincide con el nombre del controlador en notación smallcase. Por ejemplo: si posees un controlador cuya clase se denomina ``PersonalTecnicoController`` esta por convenio tiene un directorio de vistas ``personal_tecnico``.
- Cada vez que se ejecuta una acción se intenta cargar una vista cuyo nombre es el mismo que el de la acción ejecutada.
- Los templates deben ubicarse en el directorio ``views/_shared/templates``.
- Los partials deben ubicarse en el directorio ``views/_shared/partials``.
- Por defecto se utiliza el template ``default`` para mostrar las vistas de acción


Plantillas
----

En nuestro caso, es la forma de presentación de la vista. Dentro de cada plantilla se debe llamar el método ``content()`` de la clase ``View`` así:

.. code-block:: php

    <!DOCTYPE html>
    <html lang="es">
        <head>        
        </head>
        <body>
            <?php View::content(); ?>
        </body>
    </html>

Con ese llamado, renderizamos la vista del método del controlador en el template. 

Si es necesario cambiar el template para una acción o controlador, lo indicamos de la siguiente manera:

.. code-block:: php

    <?php
        
    class EjemploController extends AppController {

        public function before_filter() {
            View::template('otro_template');
        }   

        public function hola() {
        
        }
            
    } 

Si el tipo de respuesta de la vista no incluye una plantilla:

.. code-block:: php

    <?php
    
    class EjemploController extends AppController {

        public function hola() {
            if(Input::isAjax() {  //Si la petición es con Ajax se quita el template                
                View::template(NULL);
            }   
        }
            
    }  


Vistas
----

Como anteriormente se comentó, cada vez que se ejecuta una acción se intenta cargar una vista cuyo nombre es el mismo que el de la acción ejecutada.

En caso de querer cambiar el nombre de la vista que no esté asociada al nombre de la acción:

.. code-block:: php

    <?php
    
    class EjemploController extends AppController {

        public function hola() {
            View::select('saludo'); //Se cambia la vista 'hola' por defecto a 'saludo'
        }
            
    } 

En caso no querer mostrar alguna vista:

.. code-block:: php

    <?php
    
    class EjemploController extends AppController {

        public function hola() {
            View::select(NULL); //Se excluye la renderización de la vista
        }
            
    } 


Pasando datos a la vista
----

Para utilizar las variables de los controladores en las vistas, estas deben ser variables públicas ($this->nombre_variable) pues KumbiaPHP extrae esas variables y las convierte en variables normales ($nombre_variable). 

Ejemplo: 

.. code-block:: php

    <?php
    
    class EjemploController extends AppController {

        public function hola() {
            $this->usuario = 'Mundo';
        }
            
    } 


Vista: ``view/ejemplo/saludo.phtml``

.. code-block:: php

    Hola <?php echo $usuario; ?>


Mostrando búffer de salida de los controladores
----

Para mostrar el contenido del buffer de salida se hace uso del método ``View::content()``, donde el contenido del búffer de salida lo constituye principalmente los ``echo`` o ``print`` que efectúe el usuario y así mismo los mensajes ``Flash``. Al invocar ``View::content()`` se muestra el contenido del búffer de salida en el lugar donde fue invocado.

Ejemplo: 

.. code-block:: php

    <?php
    
    class EjemploController extends AppController {

        public function hola() {
            Flash::valid('Hola Mundo');
        }
            
    } 


Vista: ``view/ejemplo/saludo.phtml``

.. code-block:: php

    Saludo realizado:
    <?php View::content() ?>

