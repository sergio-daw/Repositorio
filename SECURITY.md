# Política de seguridad del repositorio

**Despliegue de Aplicaciones Web** · Sergio Segoviano Gálvez · 2º DAW

Este documento explica cómo se protege la información de este repositorio: qué ficheros no se suben, quién puede acceder y con qué permisos, qué buenas prácticas se siguen y cómo se recupera el repositorio si se pierden datos.

## Índice

1. [Protección de datos sensibles](#1-protección-de-datos-sensibles)
2. [Accesibilidad y permisos](#2-accesibilidad-y-permisos)
3. [Buenas prácticas de seguridad](#3-buenas-prácticas-de-seguridad)
4. [Recuperación ante pérdida de datos](#4-recuperación-ante-pérdida-de-datos)

## 1. Protección de datos sensibles

El fichero [`.gitignore`](.gitignore) está en la **raíz del repositorio**, por lo que sus reglas se aplican a todas las carpetas. Indica a Git qué ficheros no debe rastrear ni subir.

### Por qué se excluye cada grupo

| Grupo | Ejemplos | Motivo |
| --- | --- | --- |
| Datos sensibles | `.env`, `*.key`, `*.pem`, `credenciales*`, `config.properties`, `db.properties` | Contienen contraseñas, claves de API o certificados. Este repositorio es **público**: cualquiera podría leerlos y usarlos. |
| Compilados y binarios | `*.class`, `*.jar`, `*.war`, `build/`, `dist/`, `target/`, `out/` | Se generan al compilar y se pueden volver a crear en cualquier momento. Git no puede comparar versiones de un binario y hacen crecer mucho el repositorio. |
| Configuración local del IDE | `nbproject/private/`, `.idea/`, `.vscode/`, `*.iml` | Son ajustes de cada persona (rutas de su equipo, preferencias del editor). Si se suben, los miembros del equipo se sobrescriben la configuración entre ellos. |
| Temporales y registros | `*.log`, `*.tmp`, `*.bak`, `*~`, `*.swp`, `*.orig` | Los crean los programas mientras se trabaja. No aportan nada y los logs pueden contener rutas o datos de usuarios. |
| Bloqueos de LibreOffice y Office | `.~lock.*#`, `~$*` | Aparecen mientras un `.odt` o `.docx` está abierto. Como en este repositorio se guardan memorias en esos formatos, es fácil subirlos por error. |
| Ficheros del sistema operativo | `.DS_Store`, `Thumbs.db`, `desktop.ini` | Los crean Windows y macOS para guardar miniaturas y ajustes de carpetas. No tienen relación con el proyecto. |

### Limitaciones del `.gitignore`

- **No borra lo que ya está subido.** Si un fichero estaba en el repositorio antes de añadir la regla, Git lo sigue rastreando. Hay que quitarlo con `git rm --cached fichero`, o borrarlo desde la web de GitHub.
- **No borra el historial.** Si se sube una contraseña y después se borra, sigue visible en los commits anteriores. La única medida segura es **cambiar esa contraseña** inmediatamente.
- **Solo actúa al trabajar con Git.** Si se suben ficheros arrastrándolos en la web de GitHub (*Add file → Upload files*), el `.gitignore` no los filtra. Hay que revisar lo que se sube antes de confirmar el commit.

## 2. Accesibilidad y permisos

### Tipos de cuenta en GitHub

En un repositorio de una **cuenta personal** solo existen dos niveles: el propietario y los colaboradores, y todos los colaboradores tienen permiso de escritura. Para asignar permisos distintos (lectura, escritura, administración), el repositorio tiene que pertenecer a una **organización**. Las organizaciones son gratuitas en GitHub.

### Roles de un repositorio de organización

| Rol | Qué puede hacer |
| --- | --- |
| **Read** | Ver y clonar el repositorio, abrir issues y comentar. No puede subir cambios. |
| **Triage** | Lo mismo que Read, y además organizar issues y pull requests (etiquetas, asignaciones, cerrarlos). |
| **Write** | Subir cambios, crear ramas y fusionar pull requests. |
| **Maintain** | Lo mismo que Write, y además gestionar el repositorio sin acciones peligrosas: descripción, temas, wikis, opciones de fusión. |
| **Admin** | Control total: configuración, colaboradores, ramas protegidas, visibilidad, transferir o borrar el repositorio. |

### Resultados de las pruebas

Pruebas realizadas por un compañero con cada rol asignado:

| Prueba | Read | Write | Admin |
| --- | --- | --- | --- |
| Ver y clonar el repositorio | | | |
| Editar un fichero y hacer commit | | | |
| Subir directamente a `main` con la rama protegida | | | |
| Abrir un pull request | | | |
| Fusionar un pull request | | | |
| Ver la pestaña *Settings* | | | |

*(Completar con ✓ o ✗ según el resultado de cada prueba; las capturas están en la carpeta de evidencias.)*

### Permiso adecuado para cada rol del equipo

Se aplica el **principio de mínimo privilegio**: cada persona recibe solo el acceso que necesita para su trabajo.

| Rol en el equipo | Permiso | Motivo |
| --- | --- | --- |
| Profesor, cliente o invitado | Read | Solo necesita revisar el trabajo. |
| Tester o QA | Triage | Abre y clasifica errores, pero no cambia el código. |
| Desarrollador | Write | Sube su trabajo en ramas y abre pull requests. |
| Jefe de proyecto | Maintain | Organiza el repositorio sin poder borrarlo ni cambiar la seguridad. |
| Responsable del repositorio | Admin | Una o dos personas como máximo, para gestionar accesos y seguridad. |

## 3. Buenas prácticas de seguridad

- **Rama `main` protegida.** Nadie sube directamente a `main`: los cambios se hacen en otra rama y entran mediante un pull request. Se impide forzar el historial (*force push*) y borrar la rama. En repositorios públicos esta opción es gratuita.
- **Revisión de pull requests.** Cada pull request necesita al menos una aprobación de otra persona antes de fusionarse. Así se detectan errores y datos sensibles antes de que lleguen a `main`.
- **Autenticación en dos pasos (2FA)** activada en todas las cuentas con acceso al repositorio.
- **Mínimo privilegio** en los permisos, revisando cada cierto tiempo la lista de colaboradores y quitando el acceso a quien ya no participa.
- **Nunca subir secretos.** Las contraseñas van en ficheros excluidos por el `.gitignore`. Se puede subir un ejemplo sin valores reales, como `.env.example`.
- **Alertas de seguridad de GitHub.** En *Settings → Code security* se pueden activar *Secret scanning*, que avisa si se sube una clave conocida, y *Dependabot*, que avisa de librerías con vulnerabilidades.
- **Commits pequeños y con mensajes claros**, para saber qué cambió en cada momento y poder deshacerlo fácilmente.

## 4. Recuperación ante pérdida de datos

| Situación | Cómo se recupera |
| --- | --- |
| Un commit con errores | `git revert <commit>` crea un commit nuevo que deshace los cambios sin reescribir el historial. Desde la web, el pull request fusionado tiene un botón *Revert*. |
| Un fichero borrado por error | Se recupera de un commit anterior con `git checkout <commit> -- fichero`, o desde la web abriendo el historial del fichero. |
| Una rama borrada o un `reset` equivocado | `git reflog` guarda los movimientos recientes de cada clon local y permite volver a cualquier punto. Las ramas borradas en un pull request se pueden restaurar desde la web. |
| Pérdida del repositorio en GitHub | Cada clon local es una **copia completa** con todo el historial, así que se puede volver a subir desde cualquier equipo. GitHub también permite restaurar durante un tiempo un repositorio borrado, desde *Settings → Repositories → Deleted repositories*. |
| Copia de seguridad periódica | `git clone --mirror https://github.com/cras700/Repositorio.git` guarda una copia exacta (todas las ramas y etiquetas) en otro disco o servicio. |
| Versiones importantes | Se marcan con etiquetas (`git tag`) o *Releases* en GitHub, para volver fácilmente a una entrega concreta. |

Con la rama protegida y la revisión de pull requests se evita la mayoría de los errores graves. Con varias copias del repositorio, en GitHub, en los equipos del equipo de trabajo y en una copia espejo, se puede recuperar aunque falle cualquiera de ellas.
