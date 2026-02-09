
G-osv-scanner
Repository navigation
Code
Issues
84
 (84)
Escáner de vulnerabilidades escrito en Go que utiliza los datos proporcionados por https://osv.dev

google.github.io/osv-scanner/
Licencia
 Licencia Apache-2.0
Código de conducta
 Código de conducta
Contribuyendo
 Contribuyendo
Política de seguridad
 Política de seguridad
 8.4k estrellas
 522 horquillas
 64 mirando
 14 Branches
 56 Tags
 Actividad
 Propiedades personalizadas
Repositorio público
google/osv-scanner
Nombre	
robot osvG-Rath
robot osv
y
G-Rath
hace 5 días
.Géminis
hace 4 meses
.github
hace 3 semanas
comportamiento
hace 6 meses
comando
hace 5 días
documentos
hace 5 días
interno
hace 5 días
paquete
hace 3 semanas
guiones
hace 2 semanas
.dockerignore
hace 2 años
.editorconfig
hace 6 meses
Navegación por los archivos del repositorio
LÉAME
Código de conducta

Cuadro de mando de OpenSSF Boletín de calificaciones Go código decodificador SLSA 3 Lanzamiento de GitHub

Utilice OSV-Scanner para detectar vulnerabilidades que afecten las dependencias de su proyecto. OSV-Scanner proporciona una interfaz oficial para la base de datos de OSV y una interfaz CLI para OSV-Scalibr que conecta la lista de dependencias de un proyecto con las vulnerabilidades que las afectan.

OSV-Scanner admite una amplia gama de tipos de proyectos, administradores de paquetes y funciones, incluidos, entre otros:

Lenguajes: C/C++, Dart, Elixir, Go, Java, Javascript, PHP, Python, R, Ruby, Rust.
Gestores de paquetes: npm, pip, yarn, maven, módulos go, cargo, gem, composer, nuget y otros.
Sistemas operativos: detecta vulnerabilidades en paquetes de SO en sistemas Linux.
Contenedores: escanea imágenes de contenedores en busca de vulnerabilidades en sus imágenes base y paquetes incluidos.
Remediación guiada: proporciona recomendaciones para actualizaciones de versiones de paquetes según criterios como profundidad de dependencia, gravedad mínima, estrategia de solución y retorno de la inversión.
OSV-Scanner utiliza la biblioteca extensible OSV-Scalibr para ofrecer esta funcionalidad. Si un lenguaje o gestor de paquetes no es compatible actualmente, envíe una solicitud de funcionalidad.

Base de datos subyacente
La base de datos subyacente, OSV.dev, tiene varios beneficios en comparación con las bases de datos y escáneres de asesoramiento de código cerrado:

Cubre la mayoría de los ecosistemas de lenguajes y sistemas operativos de código abierto (incluido Git ) y es completo.
Cada aviso proviene de una fuente abierta y autorizada (por ejemplo, avisos de seguridad de GitHub , base de datos de avisos de RustSec , avisos de seguridad de Ubuntu )
Cualquiera puede sugerir mejoras a los avisos, dando como resultado una base de datos de muy alta calidad.
El formato OSV almacena de forma inequívoca información sobre las versiones afectadas en un formato legible por máquina que se asigna con precisión a la lista de paquetes de un desarrollador.
Todo lo anterior da como resultado notificaciones de vulnerabilidades precisas y procesables, lo que reduce el tiempo necesario para resolverlas. ¡Consulta OSV.dev para más detalles!

Instalación básica
Para instalar OSV-Scanner, consulte la sección de instalación de nuestra documentación. Las versiones de OSV-Scanner se encuentran en la página de versiones del repositorio de GitHub. El método recomendado es descargar un binario precompilado para su plataforma. También puede go install github.com/google/osv-scanner/v2/cmd/osv-scanner@latestcompilarlo desde el código fuente.

Características principales
Para más información, consulte nuestra documentación detallada para aprender a usar OSV-Scanner. Para obtener información detallada sobre cada función, haga clic en sus títulos en este archivo README.

Nota: Estas son las instrucciones para la versión beta más reciente de OSV-Scanner V2. Si usa la versión 1, consulte el archivo README y la documentación de la versión 1 .

Escaneando un directorio de origen
$ osv-scanner scan source -r /path/to/your/dir
Este comando escaneará recursivamente el directorio especificado en busca de archivos de paquetes compatibles, como package.json, go.mod, pom.xml, etc. y mostrará cualquier vulnerabilidad descubierta.

OSV-Scanner tiene la opción de usar el análisis de llamadas para determinar si una función vulnerable realmente se está utilizando en el proyecto, lo que genera menos falsos positivos y alertas procesables.

OSV-Scanner también puede detectar código C/C++ de terceros para el análisis de vulnerabilidades. Consulte aquí para obtener más información.

Archivos de bloqueo compatibles
OSV-Scanner es compatible con más de 11 ecosistemas lingüísticos y más de 19 tipos de archivos de bloqueo. Para comprobar si su ecosistema está cubierto, consulte nuestra documentación detallada .

Escaneo de contenedores
OSV-Scanner también admite un escaneo integral y con reconocimiento de capas de imágenes de contenedores para detectar vulnerabilidades en los siguientes paquetes de sistemas operativos y dependencias específicas del idioma.

Soporte de distribución	Compatibilidad con artefactos del lenguaje
Sistema operativo Alpine	Ir
Debian	Java
Ubuntu	Nodo
Pitón
Consulte la documentación completa para obtener detalles sobre el soporte.

Uso :

$ osv-scanner scan image my-image-name:tag
Captura de pantalla de la salida HTML del escaneo del contenedor

Escaneo de licencias
Comprueba las licencias de tus dependencias con los datos deps.dev. Para un resumen:

osv-scanner --licenses path/to/repository
Para comprobarlo con una lista de licencias permitidas (formato SPDX):

osv-scanner --licenses="MIT,Apache-2.0" path/to/directory
Escaneo sin conexión
Analice su proyecto con una base de datos OSV local. No se requiere conexión de red tras la descarga inicial de la base de datos. La base de datos también se puede descargar manualmente.

osv-scanner --offline --download-offline-databases ./path/to/your/dir
Remediación guiada (experimental)
OSV-Scanner ofrece remediación guiada, una función que sugiere actualizaciones de versiones de paquetes según criterios como la profundidad de las dependencias, la gravedad mínima, la estrategia de corrección y el retorno de la inversión. Actualmente, ofrecemos remediación de vulnerabilidades en los siguientes archivos:

Ecosistema	Formato de archivo (tipo)	Estrategias de remediación respaldadas
npm	package-lock.json(archivo de bloqueo)	in-place
npm	package.json(manifiesto)	relock
Experto	pom.xml(manifiesto)	override
Este está disponible como un comando CLI sin interfaz gráfica, así como también en modo interactivo.

Ejemplo (para npm)
$ osv-scanner fix \
    --max-depth=3 \
    --min-severity=5 \
    --ignore-dev  \
    --strategy=in-place \
    -L path/to/package-lock.json
Modo interactivo (para npm)
$ osv-scanner fix \
    -M path/to/package.json \
    -L path/to/package-lock.json
Captura de pantalla de la pantalla de resultados de bloqueo interactivo con algunos parches de relajación seleccionados

Fuentes de datos y privacidad
OSV-Scanner se comunica con los siguientes servicios externos durante su funcionamiento:

API de OSV.dev
La principal fuente de datos para la información sobre vulnerabilidades. OSV-Scanner consulta esta API para comprobar si los paquetes tienen vulnerabilidades conocidas e identificar dependencias de C/C++. Los datos enviados incluyen nombres de paquetes, versiones, ecosistemas y hashes de archivos. Utilice --offlineel modo para deshabilitar las solicitudes de red y, en su lugar, analizar con una base de datos local.

API deps.dev
Se utiliza para obtener información complementaria del paquete:

Resolución de dependencias : resuelve gráficos de dependencias para el escaneo y la remediación de vulnerabilidades.
Escaneo de imágenes de contenedores : consulta los metadatos de las imágenes de contenedores para detectar vulnerabilidades.
Escaneo de licencias ( --licensesbandera): recupera información de licencia para paquetes
Obsolescencia de paquetes : Comprueba si los paquetes están obsoletos
Los datos enviados incluyen nombres de paquetes, versiones y ecosistemas. No se transmite el código fuente.

Registros de paquetes
Al utilizar el registro nativo para la resolución de dependencias (en lugar de deps.dev), OSV-Scanner puede consultar:

Registro	URL	Usado para
Centro de Maven	repo.maven.apache.org/maven2	Metadatos del paquete Maven y archivos POM
Registro npm	registry.npmjs.org	metadatos del paquete npm
PyPI	pypi.org	Metadatos del paquete Python
Contribuir
Informar problemas
Si encuentras algo que parezca un error, utiliza el sistema de seguimiento de incidencias de GitHub . Antes de reportar una incidencia, busca incidencias existentes para ver si ya está cubierta.

Contribuyendo con código aosv-scanner
Consulte CONTRIBUTING.md para obtener documentación sobre cómo contribuir con código.

Historia de las estrellas
Gráfico de la historia de las estrellas
