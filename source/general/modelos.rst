Los Modelos
====   

.. contents:: Los modelos son la lógica de negocio y todo lo que tiene que ver con el acceso a la base de datos. Los modelos son ubicados dentro de la carpeta ``models`` de nuestra aplicación y el archivo se deben llamar igual que el nombre de la tabla ej: cliente.php, tipo_cliente.php. Como convención de la base de datos las llaves primarias se deben llamar ``id`` y deben ser ``AUTOINCREMENT`` o ``SERIAL``, los campos terminados en ``_id`` se utilizan para relaciones foráneas a otras tablas, ejemplo ``libro_id``, si se desea almacenar la fecha de registro y modificación se pueden crear los campos ``*_at`` para el registro y ``*_in`` para la modificación: registrado_at, modificado_in. Adicionalmente deben ser typo ``DATETIME``, el cual se carga el valor automáticamente.


Como usar los Modelos
----

Los Modelos representan la lógica de la aplicación, y son parte fundamental para el momento que se desarrolla una aplicación, un buen uso de estos nos proporciona un gran poder al momento que se necesita escalar, mantener y reusar código en una aplicación. Por lo general un mal uso de los modelos es solo dejar el archivo con la declaración de la clase y toda la lógica se genera en el controlador. Esta práctica trae como consecuencia que en primer lugar el controlador sea dificilmente entendible por alguien que intente agregar y/o modificar algo en esa funcionalidad, en segundo lugar lo poco que puedes rehusar el código en otros controladores y lo que hace es repetirse el código que hace lo mismo en otro controlador. Partiendo de este principio los controladores NO deberían contener ningún tipo de lógica solo se encargan de atender las peticiones del usuarios y solicitar dicha información a los modelos con esto garantizamos un buen uso del MVC.


Definiendo un Modelo
----

La clase de cada modelo se debe llamar asi:

.. code-block:: php

    <?php
    
    class Tabla extends ActiveRecord {
            
    }

Invocando un modelo en nuestros controladores
----

En KumbiaPHP existen 2 formas de invocar nuestros modelos según nuestros casos requeridos

- ``Load::models('nombre_modelo', 'nombre_otro_modelo');`` 
    De esta manera incluimos el (los) archivo(s) correspondiente(s) al (los) modelo(s) indicados(s).  Esta metodología es útil si necesitamos utilizar el modelo en mas de una acción dentro de uno o varios controladores.    
- ``Load::model('nombre_modelo');`` 
    De esta manera devuelve un objeto creado del modelo indicado.  Esta metodología es útil cuando solo hacemos uso de un modelo en un solo método o acción dentro de un controlador.
    
