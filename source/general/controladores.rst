Los Controladores
====

.. contents:: Los controladores son los encargados de recibir las peticiones de los usuarios y decidir qué hacer con ellas mismas. Los controladores se ubican en la carpeta controllers de tu app y se deben llamar ejemplo_controller.php.

Definiendo un Controlador
----

La clase de cada controlador se debe llamar asi:

.. code-block:: php

    <?php
    
    class EjemploController extends AppController {
            
    }

Acciones
----

Los controladores están compuestos por métodos o acciones que son invocados de las peticiones realizadas por el usuario. Cada método puede tener una vista correspondiente y puede interactuar con uno o varios modelos.

.. code-block:: php

    <?php
    
    class EjemploController extends AppController {

        public function hola() {

        }
            
    }

Las URLs de KumbiaPHP están caracterizadas por tener varias partes, cada una de ellas con una función conocida. Para obtener desde un controlador los valores que vienen en la URL podemos usar algunas propiedades definidas en el controlador.

Tomemos por ejemplo esta URL:

``http://www.dominio.com/noticia/ver/12/``

Analizando tenemos:

- *Controlador:* noticia
- *Acción:* ver
- *Parámetros:* 12


Para este ejemplo podemos armar nuestro controlador de la siguiente manera:

.. code-block:: php

    <?php
    
    class NoticiaController extends AppController {

        public function ver($id) {
            echo $this->controller_name; //Variable que contiene el nombre del controlador ejecutado
            echo $this->action_name; //Variable que contiene el nombre del método del controlador ejecutado            
            var_dump($this->parameters); //Un array con todos los parámetros enviados al métdo
        }
            
    }
    

En el ejemplo anterior se definió en la acción ``ver($id)`` con un solo parámetro, esto quiere decir que si no se envía ese parámetro o se intentan enviar más parámetros adicionales KumbiaPHP lanzará una exception (en producción mostrará un error 404). Este comportamiento es por defecto en el framework y se puede cambiar para determinados escenarios según el propósito de nuestra aplicación para la ejecución de una acción.

Para quitar esta validación, indicamos que descarte el número de parámetros que pasan por la URL:

.. code-block:: php

    <?php
    
    class NoticiaController extends AppController {

        /**
        * Limita la cantidad correcta de parámetros de una acción
        */        
        public $limit_params = FALSE;

        //Métodos
            
    }

Acciones por defecto
----

Tomemos por ejemplo esta URL:

``http://www.dominio.com/noticia/``

Como podemos observar, hemos definido solamente el controlador ``noticia``.  KumbiaPHP analiza el controlador y tomará por defecto la acción ``index``:

.. code-block:: php

    <?php
    
    class NoticiaController extends AppController {
    
        /**
        * Método por defecto, si no se ha definido en la url
        */
        public function index() {
            
        }
    
            
    }



Filtros y Callback
----

Cada controlador tiene una serie de filtros y callback que se ejecuta antes/después de cualquier método o acción. Es ideal para el manejo de sesiones, cambios de vistas entre otras.

La super clase ``AppController`` posee 2 métodos que se ejecutan al inicializar y finalizar cualquier controlador. Ver el archivo ``app_controller.php`` dentro de la carpeta ``libs`` de la aplicación.

Los controladores definidos por nosotros poseen un método que se ejecuta antes de inicializar y finalizar cualquier acción. Podemos invocar el callback de la siguiente manera:

.. code-block:: php

    <?php
    
    class NoticiaController extends AppController {

        /**
        * Callback que se ejecuta antes de ejecutar la acción
        */ 
        public function before_filter() {
            echo "Se va a ejecutar la acción $this->action_name";
            return TRUE; //Esto es opcional, pero si retorna FALSE detendrá la ejecución de la acción. Adicionalmente podemos enrutar a otra acción o controlador
        }

        public function ver($id) {
            
        }

        /**
        * Callback que se ejecuta despues de ejecutar la acción
        */ 
        public function after_filter() {
            echo "Se ha ejecutado la acción $this->action_name";
        }
            
    }
