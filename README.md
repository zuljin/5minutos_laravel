### Resumen sobre DDD. Domain Driven Design.

Resumen para introducirse en el desarrollo orientado a dominios. Esta es una interpretación personal de la información que he leído en varios lugares. Una de mis mayores fuentes de información ha sido el libro Domain Driven Design Quickly.

La mejor forma de entender los conceptos de este esquema de desarrollo, es olvidar por un momento todo lo que conoces sobre programación y pensar en el mundo real. La OOP ha sido abstraída del mundo real y, por tanto, es una representación de este. Del mismo modo, el DDD es una abstracción de cómo hemos estructurado el mundo real y buscar similitudes entre ambos te facilitará la tarea de entenderlo. 

Dado que actualmente estoy desarrollando usando el Framework Laravel, he incluido algunos ejemplos relacionados con este Framework pero son perfectamente aplicables a cualquier otro framework/lenguaje de programación.

DDD no define implementaciones, define conceptos y 'reglas de implementación'. Por tanto es independiente de cualquier framework y lenguaje.

>**Nota:** Esta entrada continúa en desarrollo.

#### Lenguaje Ubicuo: Ubiquitous Language

El lenguaje ubicuo es un término que introdujo Eric Evans en su libro sobre DDD (__Domain Driven Design__) como propuesta para crear un lenguaje común entre los programadores y los usuarios.

La definición propone nombrar las variables, métodos y clases con lenguaje del dominio de modo que sea 'autoexplicable'.

Con este tipo de nombres, el código se documenta por sí mismo:
- `getRandomEntityFrom($modelName)` devuelve una entidad aleatoria a partir del nombre de un modelo.
- Comprueba si el modelo existe en la colección antes de acceder a él.
- `createMultipleEntities($numberOfEntities)` una cantidad de entidades determinada por el parámetro que recibe.
- Para ello usa un bucle en el que se crea cada vez una nueva entidad a partir del contador actual.

Habitualmente los nombres representan objetos y los verbos métodos.

#### Capas de la arquitectura

Se proponen cuatro capas conceptuales:

- Interface de usuario (User Interface)

 Responsable de presentar la información al usuario, interpretar sus acciones y enviarlas a la aplicación.
 
- Aplicación (Application)

 Responsable de coordinar todos los elementos de la aplicación. No contiene lógica de negocio ni mantiene el estado de los objetos de negocio. Es responsable de mantener el estado de la aplicación y del flujo de esta.
 
- Dominio (Domain)

 Contiene la información sobre el Dominio. Es el núcleo de la parte de la aplicación que contiene las reglas de negocio. Es responsable de mantener el estado de los objetos de negocio. (La persistencia de estos objetos se delega en la capa de infraestructura.
 
- Infraestructura (Infraestructure)

 Esta capa es la capa de soporte para el resto de capas. Provee la comunicación entre las otras capas, implementa la persistencia de los objetos de negocio y las librerías de soporte para las otras capas (Interface, Comunicación, Almacenamiento, etc..)
 
Dado que son capas conceptuales, su implementación puede ser muy variada y en una misma aplicación, tendremos partes o componentes que formen parte de cada una de estas capas. Por ejemplo, en una aplicación web desarrollada con Laravel, Las vistas formarían parte de la capa de Interface, pero Sass o Less, por ejemplo, serían parte de la infraestructura. Algunos componentes del Framework formarían parte de la infraestructura (Eloquent, Caches, etc...) y otros componentes formarían parte de la aplicación (Controladores, Comandos, Eventos, etc..). Los modelos, por ejemplo, formarían parte de la capa de Dominio.  

#### Entidades (Entities).

Cualquier objeto del dominio que mantiene un estado y comportamiento más allá de la ejecución de la aplicación y que necesita ser distinguido de otro que tenga las mismas propiedades y comportamientos, es una Entidad. A cada instancia, por tanto, se le debe asignar un identificador único. 

Por tanto, simplificando, podríamos decir que podemos definir como identidad a todos los objetos de la aplicación que tengan un identificador único.

Qué se considere entidad dependerá principalmente de la función, objetivo y contexto de la aplicación. Lo que para una aplicación puede ser una entidad, para otra podría no serlo. Habitualmente las entidades serán objetos con identidad propia en el mundo real.

Por ejemplo, en el mundo real, dos personas con el mismo nombre, edad y características, son dos personas distintas y por ello se inventó el Documento de Identidad, o DNI, Pasaporte, etc...

Para mí, los árboles no representan entidades, dado que no podría reconocer uno de otro. Pero para un jardinero encargado de catalogar todos los árboles de un parque, si la tienen y por ello su primera función será crear un método de clave única para identificar cada árbol.

Los coches tienen matrícula y, por tanto se puede realizar seguimiento de por vida de cada coche. Incluso los motores tienen un número único. Sin embargo, las alfombrillas de un coche o los espejos retrovisores no las tienen. Son meros 'objetos' que para mí, no llegan a convertirse en Entidades. Sin embargo, en una fábrica es posible que les hagan seguimiento individual y tengan asignado un número de serie interno, convirtiéndolos en entidades.

Por tanto, Entidad es un término conceptual cuya concreción a la hora de definir en nuestra aplicación qué objetos o clases se tratarán como tales, dependerá del contexto y los objetivos de esta.

Durante el desarrollo de un aplicación, al avanzar el conocimiento profundo del modelo de negocio, habrá objetos que puedan pasar a convertirse en entidades y, también sería posible lo contrario.

En una aplicación habitual de gestión de usuarios, el País es una mera etiqueta, por lo que no llega apenas a ser un objeto. Sin embargo, si aplicamos gastos de envío por país, empieza a convertirse en un objeto y si queremos hacer un seguimiento de los países por los que pasa el envío, se convertiría en Entidad. En una aplicación demográfica con encuestas globales, ocurre lo contrario, el País es una entidad y el usuario puede llegar a convertirse en un simple número.

Pueden contener otras entidades y objetos de valor.

> Habitualmente, los nombres que aparezcan en las reglas de negocio se convertirán en entidades o en objetos de valor.

#### Objetos de valor (Value Objects).

Cuando un objeto no llega a ser entidad, se le considera un objeto de valor. Los objetos de valor dentro de una aplicación son indistinguibles unos de otros. Consisten únicamente en un conjunto de propiedades y comportamientos pero no mantienen identidad alguna. Sí es posible que mantengan un estado, pero este se pierde al terminar la aplicación.

Se pueden duplicar y destruir con facilidad. Y no deberían ser modificados una vez han sido creados. 

Pueden proporcionarse a capas fuera del dominio, dado que su posible modificación no afectaría a la aplicación.

Pueden contener a su vez otros objetos de valor.

En ocasiones es posible agrupar varios atributos en un objeto de valor.

Ejemplos: 

- Dirección de un usuario podría ser un objeto de valor que contenga: Calle, Ciudad, Estado, País. A su vez, la calle podría ser un objeto de valor que contenga: Nombre de la calle, número del portal, piso, escalera, letra, etc..

- Un número de teléfono podría componerse de prefijo internacional y número. (Y compañía telefónica)

#### Servicios (Services).

La mayoría de los verbos del lenguaje ubicuo se convertirán en métodos de los objetos de la capa de negocios. Pero hay verbos o comportamientos que no es fácil concretar a cual de los objetos corresponden. Esos comportamientos suelen ser en realidad Servicios.

Volviendo al ejemplo del mundo real, la reparación del automóvil, ¿A quién corresponde?, ¿Al usuario/conductor o al vehículo? Cuando envío un paquete postal, ¿Quién hace el envío? ¿El remitente o el destinatario? En realidad, son 'servicios' que contratamos. Por que no los 'implementamos' nosotros, simplemente los usamos. Y no forman parte de nuestra vida ni entorno personal. De hecho, es posible que en cada ocasión que tengamos que reparar el coche, utilicemos un taller distinto. O que elijamos el modo y la empresa para enviar el paquete postal en función de las necesidades concretas de cada envío.

Sin embargo, si soy el dueño de un taller de vehículos o una empresa de mensajería, esos servicios son el núcleo de mi aplicación y de mi modelo de negocio. 

En el mundo de la programación sucede exactamente lo mismo. Cuando encuentres comportamientos que no 'pertenecen' a ninguna entidad porque simplemente son 'utilizados' por estas, conviértelos en servicios.

Habitualmente los servicios actuarán como interface o medio de relacionarse de varios objetos. Podemos tener servicios en cualquiera de las capas de la aplicación.

En Laravel, los controladores son claros ejemplos de servicios. Así como la gestión de la caché, de la configuración de la aplicación, el envío de correos, etc..

Algunas claves para distinguirlos:

- Son comportamientos que no pertenecen a ninguna entidad.
- Pueden intercambiarse con facilidad por servicios de similares características.
- Van a ser usados por varias entidades en distintos puntos de la aplicación.
- Nos interesa más el resultado del proceso que el proceso en sí.
- Desde nuestro punto de vista, no tienen un estado interno (o nos es desconocido y no nos interesa).

> Habitualmente, los verbos que aparezcan en las reglas de negocio se convertirán en servicios o en métodos de las entidades u objetos de valor.

#### Módulos (Modules).

Los módulos permiten seguir con facilidad el concepto de 'acoplamientos débiles y alta cohesión'. Cuando una aplicación comienza a ser compleja, es preferible dividirla en módulos.

En la vida real puedes ver esa división en las carpetas de tu disco duro. Empiezas con una carpeta llamada fotos y al cabo de un tiempo, organizas tus fotos por años, por eventos, o por ambos al mismo tiempo. También puedes ver módulos en los departamentos de una empresa en comparación con un freelance o una startup de cuatro o cinco empleados. O en la división de los espacios en una casa de 20 o 30 metros cuadrados y de un chalet de 400 metros cuadrados y cuatro plantas. (Como el mío (en sueños, claro), jajaja!)

En el mundo de la programación, las bases de datos son módulos que contienen a su vez tablas relacionadas entre sí de algún modo concreto. Los paquetes que instalas en Laravel, son módulos independientes entre sí pero que pueden interactuar entre ellos. 'Componente' es un buen sinónimo de 'Módulo' y Laravel contiene muchos componentes: 'Pagination', 'FileSystem', 'Encryption', etc...

Organiza tus módulos agrupando clases o bien por que se comunican entre ellas o, preferiblemente, o porque están relacionadas a nivel funcional.

Dado que los módulos suelen contener varios objetos, es preferible implementar un interface para que los demás módulos no accedan directamente a los objetos.

Si un módulo comienza a volverse complejo durante el desarrollo, considera refactorizarlo y convertirlo a su vez en varios módulos más pequeños.

#### Agregados (Aggregates).

Mientras los módulos suelen estar formados de clases relacionadas con los servicios o la funcionalidad de la aplicación, los agregados son grupos de entidades relacionadas entre sí a nivel de negocio.

Cuando tenemos varias entidades que están relacionadas entre sí y son dependientes entre ellas, las agrupamos en Agregados. 

En un agregado, se define cual va a ser la entidad raíz ('root') o principal y se dará acceso público únicamente a esta. De modo que las entidades externas sólo podrán acceder a las entidades internas a través de la entidad raíz.

Por ejemplo, todos los objetos que tengo en casa son dependientes de mí, y nadie tiene acceso público a ellos. Si alguien quiere usar mi ordenador, o que le preste un libro, tiene que pedírmelo a mí. Formamos un agregado y yo soy la entidad raíz. Sin embargo, en algunas bibliotecas, los libros son entidades públicas por sí mismas y en otras, el acceso es a través del bibliotecario.

En el ejemplo del coche, no es lógico que cualquiera pueda utilizar o modificar mi coche o el motor de este. Lo habitual es que accedan a él a través de mí.

En las aplicaciones desarrolladas en Laravel, no accedemos al fichero de configuración de la aplicación directamente, ni a los ficheros físicos de las plantillas, lo hacemos a través de los métodos correspondientes implementados en la clase Application y View. Estos son ejemplos claros de 'acceso controlado' en módulos o paquetes. A nivel de negocio, en una aplicación que gestione una biblioteca, si tenemos un agregado con la clase Usuario y una colección de los libros que el usuario tienen en préstamo, no sería lógico que la clase Biblioteca acceda directamente a esos libros. Lo lógico es que use el método `$user->getBooksOnLoan()` en lugar de `$user->collectionBooksOnLoan`.

En el ejemplo anterior de la dirección del Usuario, tendríamos un agregado que incluiría la clase Usuario, la clase Dirección y la clase Portal. Si el interface permite que el usuario modifique su dirección, la implementación correcta en este caso sería: `$user->updateAddress($newAddress)`.

#### Factorías (Factories).

Cuando la creación de una entidad o un agregado se convierte en un proceso complejo, delegamos esa responsabilidad en una Factoría. Las factorías son clases encargadas de crear entidades o agregados, construyendo todas las entidades contenidas en ellos. Son útiles especialmente cuando las reglas de creación de estas entidades o agregados son complejas.

Las Factorías permiten abstraer y separar la lógica y reglas de creación de una entidad y dejar en las entidades únicamente las reglas de negocio que son inherentes a ellas.

De este modo, cumplimos con el Principio de Simple Responsabilidad delegando la creación de la entidad fuera de esta y dejándole únicamente aquellas responsabilidades que fon parte fundamental de sus reglas de negocio.

Es importante que el proceso de creación sea atómico y que se lance una excepción en el caso de que haya un error durante el proceso en lugar de devolver un objeto erróneo o incompleto.

En el mundo real, solemos ir a una panadería a comprar el pan en lugar de hacerlo nosotros. Y, a veces, las propias panaderías son meras distribuidoras que compran el pan a un horno de pan. Si somos freelances especializados en el Backend y nos piden una página web sencilla, podemos hacer nosotros mismos el html de la página, pero si es compleja, lo solemos encargar a una 'Factoría' automática como 'WordPress' o a un diseñador web. Es posible que el diseñador web actúe como 'Base de datos' y sea el responsable de la persistencia y de 'recrear' el diseño cada vez que sea necesario. En ese caso, se quedará con el fichero de Photoshop y nos entregará únicamente los ficheros html y gráficos jpeg. De ese modo, cada vez que necesitemos una modificación, él actuará de nuevo como Factoría.

Hay varios patrones para implementar una Factoría. Un ejemplo en Laravel, es `Illuminate\Foundation\Application` que actúa como Factoría y Contenedor. Un modelo Eloquent también actúa como factoría, abstrayendo la complejidad de crear un modelo a partir de tablas que están relacionadas entre sí.

Es posible que una Factoría utilice a su vez varias factorías para crear las entidades necesarias.

Por otro lado, hay que tener en cuenta que no es lo mismo crear un objeto desde cero que recuperarlo desde algún mecanismo de persistencia y reconstruirlo. En función de la implementación, este proceso puede realizarse un una única Factoría o en una para la creación y en otra para la reconstrucción y recuperación desde el mecanismo de persistencia.

También es importante tener en cuenta que no es igual la complejidad de una Factoría de Objetos de Valor que de Entidades.

Si el proceso de creación no es complejo, es preferible utilizar en su lugar un constructor:

- Si el proceso no es complejo.
- La creación de un objeto no involucra la creación de otros y todos los atributos necesarios se le facilitan en el constructor.
- El proceso de implementación forma parte de las reglas de negocio y responsabilidades de la Entidad o del Cliente, ya que quieren elegir la Estrategia de implementación. En este caso, usaríamos el patrón Estrategia (Strategy) en lugar del patrón Factoría.
- La clase es sencilla y no hay herencia.

#### Repositorios (Repository).

En una aplicación desarrollada siguiendo los conceptos de la OOP, la aplicación necesita mantener un puntero a cada objeto o entidad creadas. El Repositorio es el responsable de proporcionar a la aplicación esos punteros. De ese modo, es el Repositorio quien conoce qué mecanismo se está utilizando para implementar la persistencia. En algunas implementaciones el repositorio actuará también como 'Factoría', mientras en otras hará de intermediario entre estas y la capa de negocio de la aplicación.

A nivel global, el Repositorio actúa como la capa de persistencia de la aplicación. Sin embargo, este a su vez, puede utilizar otros servicios que implementen ese tipo de persistencia y utilizar uno u otro en función de una estrategia. Por ejemplo, utilizar una Caché o acceder a la Base de Datos si el objeto solicitado no está en la Caché.

Implementa repositorios para acceder únicamente a las Entidades Raíz de los Agregados. Un Repositorio puede contener interfaces con métodos que permitan solicitar un puntero o instancia de una Entidad, de varias, utilizando filtros si es necesario y proporcionando a los métodos habituales de persistencia tales como inserción, modificación y borrado. Pero su implementación debería ser sencilla y actuar como intermediario de otros Servicios o Factorías que implementen la complejidad de esos procesos.

Habitualmente, el interface de un Repositorio será similar al del sistema de persistencia utilizado. Por eso, en ocasiones, un Repositorio estará 'fuertemente acoplado' a los sistemas de persistencia y almacenamiento utilizados. Permitiendo de ese modo que la capa de negocio y la aplicación, sin embargo, estén 'débilmente acopladas' a esos sistemas.

Aunque un Repositorio puede ser visto como una Factoría, cuando se le solicite una entidad que no exista en el sistema de persistencia, debería delegar la responsabilidad de crear la nueva entidad a una Factoría y devolver la instancia o el objeto que reciba de esta.


#### Fuentes:

[Libro gratuito: Domain Driven Design Quickly](http://www.infoq.com/minibooks/domain-driven-design-quickly)
