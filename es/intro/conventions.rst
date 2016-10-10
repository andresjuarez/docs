Convenciones de CakePHP
###################

Somos grandes fans de las convenciones en la configuración. Mientras aprender las convenciones
de CakePHP toma un poco de tiempo, es mucho el tiempo que te ahorras en largas corridas.
Siguiendo las convenciones, obtienes libre funcionalidad, y te liberas a ti mismo
de la pesadilla del mantenimiento de los archivos de configuración. Las convenciones
hacen también que se logre una experiencia de desarrollo uniforme, permitiendo a otros
desarrolladores colaborar y ayudar.

Convenciones de controlladores
======================

Los nombres de las clases de los controladores son en plural, al estilo
CamelCased y agregnado al final la palabra ``Controller``. 
``UsersController`` y ``ArticleCategoriesController`` ambos son ejemplos 
de nombres de controladores siguiendo la convención.

Los métodos públicos en Controladores son a menudo expuestos como 'acciones'
accesibles a través del navegador web. Por ejemplo ``/users/view``
lleva al método ``view()`` del controlador ``UsersController`` por defecto. 
Los métodos protegidos o privados no pueden ser accedidos por routing.

Consideraciones URL para los nombres de controladores
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Como acabas de ver, controladores de una palabra, mapean a un simple path URL
en minúscula (lower case). Por ejemplo, ``UsersController`` (que debe ser definido
en el archivo de nombre **UsersController.php**) es accesado desde 
http://example.com/users.

Mientras tu puedes definir las rutas de controladores de multiples palabras
en la manera que gustes, la convención en estos casos es que tus URLs sean
en minúsculas y separada por guiones usando la clase ``DashedRoute``, por 
lo tanto ``/article-categories/view-all` is la manera correcta de acceder
a la acción: ``ArticleCategoriesController::viewAll()``

Cuando creas links utilizando ``this->Html->link()``, puedes usar las siguientes
convenciones para el arreglo url:

    $this->Html->link('link-title', [
        'prefix' => 'MyPrefix' // CamelCased
        'plugin' => 'MyPlugin', // CamelCased
        'controller' => 'ControllerName', // CamelCased
        'action' => 'actionName' // camelBacked
    ]

Para mas informacion en URLs CakePHP y manejo de parámetros, ver
:ref:`routes-configuration`.

.. _file-and-classname-conventions:

Convenciones de nombres de Archivos y Clases
===============================

En general, los nombres de archivos concuerdan con los nombres de las clases,
y siguen los estándares PSR-0 o PSR-4 para la auto-carga. Los siguientes son 
algunos ejemplos de nombres de clases y sus nombres de archivos:

-  la clase controller ``LatestArticlesController`` se encontraría en el
   archivo de nombre **LatestArticlesController.php**
-  La clase component ``MyHandyComponent`` se encontraría en el archivo
   de nombre **MyHandyComponent.php**
-  La clase table ``OptionValuesTable`` se encontraría en el archivo de
   nombre **OptionValuesTable.php**.
-  La clase Entity ``OptionValue`` se encontraría en el archivo de nombre
   **OptionValue.php**.
-  La clase behavior ``EspeciallyFunkableBehavior`` se encontraría en el
   archivo de nombre **EspeciallyFunkableBehavior.php**
-  La clase view ``SuperSimpleView`` se encontraría en el archivo de nombre
   **SuperSimpleView.php**
-  La clase helper ``BestEverHelper`` se encontraría en el archivo de nombre
   **BestEverHelper.php**

Cada archivo debe ser alojado en el folder/namespace apropiado en tu app
folder.

.. _model-and-database-conventions:

Convenciones para Modelos y Bases de Datos
==========================================

Los nombres de las clases Table son en plural y en formato 
CamelCased. ``Users``, ``ArticleCategories`` y ``UserFavoritePages``
son ejemplos de  nombres de modelos siguiendo la convención.

Los nombre de las tablas corresponden a los modelos de CakePHP son
en plural y separados por underscore. Las tablas correspodientes a 
los modelos mencionados anteriormente deberian ser ``users``,
``article_categories`` y ``user_favorite_pages``, respectivamente.

La convención es usar palabras en Inglés para el nombre de las tablas 
y las columnas. Si utiliza palabras en otro idioma, CakePHP podría no
ser capaz de procesar de manera correcta las infleciones (de singular 
a plural y viceversa). Si necesita agregar sus propias reglas para su 
idioma para algunas palabras, puede usar la clase utility 
:php:class:`Cake\\Utility\\Inflector`. Además de definir estas reglas de
inflección personalizadas, esta clase tambien le permite comprobar que
CakePHP entiende su sintaxis personalizada para palabras plurales y 
singulares- Ver la documentación acerca de este tema
:doc:`/core-libraries/inflector` para mayor información.

Nombre de campos con dos o mas palabras deber ser serparas con 
underscore: ``first_name``.

Claver foráneas en relaciones hasMany, belongsTo/hasOne son reconocidas por
defecto como el nombre (en sigular) de la tabla relacionada seguida de ``_id``.
Entonces si Users hasMany Articles, la tabla ``articles`` tendrá una referencia
a la tabla  ``users`` a traves de una clave foránea ``user_id``. Para una tabla
como ``article_categories`` cuyo nombre contiene multiples palabras, la clave
foránea deberá ser ``article_category_id``.

Tablas join, usadas en relaciones BelogsToMany entre modelos, deben ser nombradas
despues de los modelos cuyas tablas hacen el join, ordenadas alfabéticamente
(``articles_tags`` antes que ``tags_articles``).

Además hacer uso de claves primarias autoincrementales, también deberias utilizar 
columnas UUID. CakePHP creará una clave unica de 36 caracteres UUID 
(:php:meth:`Cake\Utility\Text::uuid()`) siempre que guardes un nuevo record
utilizando el método ``Table::save()``.


Convenciones de Vistas
======================

Los archivos de las vistas son nombrados despues de la función del controlador
que ellos despliegan, separados con underscore. La función ``viewAll()`` del
controlador ``ArticlesController`` será una vista del archivo en 
 **src/Template/Articles/view_all.ctp**.

El patrón básico es
**src/Template/Controller/underscored_function_name.ctp**.

Al nombrar las piezas de su aplicación usando las convenciones de CakePHP,
obtienes funcionalidad sin la molestia ni las trabas del mantenimiento de
configuración. Aqui hay un ejemplo final que se obtiene al seguir las 
convenciones juntas:

-  Tabla base de datos: "articles"
-  Clase Table: ``ArticlesTable``, encontrado en **src/Model/Table/ArticlesTable.php**
-  Clase Entity: ``Article``, encontrado en **src/Model/Entity/Article.php**
-  Clase Controller: ``ArticlesController``, encontrado en
   **src/Controller/ArticlesController.php**
-  Vista, encontrado en **src/Template/Articles/index.ctp**

Usando estas convenciones, CakePHP sabe que una solicitud a 
http://example.com/articles/ mapea una llamada a la funcion ``index()`` del
controlador ArticlesController, donde el modelo Articles es automáticamente
disponible (y automáticamente vinculado a la tabla 'articles' en la base
de datos), y reproduciodo en un archivo. Ninguna de estas relaciones se 
han configurado por cualquier otro medio distinto que mediante a la creación
de clases y archivos que se necesitaría para crear de todos modos.

Ahora que conoces los fundamentos de CakePHP, deberías intentar una carrera a 
traves de :doc:`/tutorials-and-examples/bookmarks/intro` para ver como encajan
las cosas.

.. meta::
    :title lang=es: CakePHP Conventions
    :keywords lang=es: web development experience,maintenance nightmare,index method,legacy systems,method names,php class,uniform system,config files,tenets,articles,conventions,conventional controller,best practices,maps,visibility,news articles,functionality,logic,cakephp,developers
