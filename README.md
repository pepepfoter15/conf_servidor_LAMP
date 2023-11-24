# 1ª Tarea: Vagrant
Queremos automatizar la creación de un servidor LAMP usando Vagrant, el esquema que queremos desarrollar, que vemos en la imagen, tiene las siguientes características:

Es escenario tiene tres máquinas:

router: que está conectada a una red pública (br0) y a una red privada (muy aislada) red_intra. La máquina router debe salir por la red pública. Esta máquina no va a utilizar eth0 para acceder al exterior.

web: En este servidor se instalará un servidor web. Esta máquina está conectada al router por la red_intra. Y esta conectada por otra red muy aislada red_datos a la máquina bd.

bd: En este servidor se instalará un gestor de bases de datos. Esta máquina está conectada al router por la red_intra. Y esta conectada a la máquina web por la red privada muy aislada red_datos.

# 2ª Tarea: AnsiblePermalink
Vamos a usar la interfaz eth0 de las máquinas que están conectadas a la red de mantenimiento para conectarnos con ansible y configurar las máquinas. Cuando termine el ejercicio, no usaremos vagrant ssh para conectarnos por ssh a las máquinas (está conexión utiliza eth0), usaremos las otras interfaces para realizar las conexiones ssh.

Queremos configurar el escenario con ansible, para que cumpla lo siguiente:

Las máquinas conectadas a la red privada muy aislada red_intra a la que está conectada el router deben tener acceso a internet. Para ello, la máquina router debe estar configurada para enrutar las peticiones de las máquinas conectadas a la red red_intra.
La máquina web tendrá un servidor web apache2 instalado. La máquina router hará DNAT para que podamos acceder a la página usando su IP pública.
La máquina bd tendrá un servidor de base de datos mariadb.
Todos los datos necesarios para ejecutar el playbook (interfaces de red, direcciones ip, nombre de usuarios, nombre de bas e de datos,…) deben estar guardados en variables.
La receta ansible debe tener al menos 5 roles:

### commons
Estas tareas se deben ejecutar en todos los nodos: actualizar los paquetes y añadir tu clave pública a la máquinas para poder acceder a ellas con ssh. ¿Existe algún módulo de ansible que te permita copiar claves públicas?.

### router
Todas las tareas necesarias para configurar router: está máquina tiene que hacer SNAT, y salir a internet por eth1 (la red pública). Las configuraciones deben ser permanentes. ¿Existe algún módulo de ansible que te permita ejecutar sysctl?. Además, como hemos dicho, tiene que hacer DNAT para poder acceder a la página del servidor web.

### redinterna
Todas las tareas que hay ejecutar en las máquinas conectadas al router por la red privada muy aislada red_intra. En concreto, debéis cambiar la ruta por defecto.

### web
Las tareas necesarias para instalar y configurar un servidor web con una página estática en la máquina web. Se debe crear un VirtualHost que se acceda con el nombre practica-tunombre.dominio.algo, para ello se deben hacer las siguientes tareas:

Configurar el nuevo VirtualHost (utilizar un template). Todos los datos del nuevo VirtualHost deben estar guardados en variables.
Activar el VirtualHost. ¿Existe algún módulo de ansible que nos permita hacerlo?
Crear el DocumentRoot.
Copiar una página estática al DocumentRoot.
Activar el módulo rewrite. ¿Existe algún módulo de ansible que nos permita hacerlo?
Si es necesario (handler) reiniciar el servicio.
Además desde la máquina webvamos a acceder a la máquina bd por la red_datos, para ello vamos a usar a nombre a esta última máquina como bd-tunombre.dominio.algo. Por lo tanto añade a la resolución estática de la máquina web la resolución correspondiente.

### mariadb
Las tareas necesarias para instalar y configurar un servidor de base de datos mariadb en la máquina bd. Se creará una base de datos y un usuario con permisos para acceder a ella. Los datos necesarios estarán guardados en variables en el playbook de ansible.

Recuerda reiniciar el servicio si es necesario (handler) al cambiar la configuración del servicio.