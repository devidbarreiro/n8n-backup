# N8N Workflow Backup

Este repositorio se utiliza para guardar una copia de seguridad automática de todos los flujos de trabajo de n8n. Utilizando un flujo de trabajo de n8n, se hace un "snapshot" de cada workflow y se guarda como un archivo JSON en este repositorio. Esto permite tener un control de versiones de todos los flujos de trabajo.

## Características

* **Copias de seguridad automáticas:** El flujo de trabajo principal está configurado para ejecutarse en un horario regular.
* **Control de versiones:** GitHub mantiene el historial de cambios de cada archivo, permitiendo ver quién hizo un cambio, cuándo y qué se modificó.
* **Organización por etiquetas:** Los flujos de trabajo se organizan en subcarpetas según las etiquetas que tengan asignadas en n8n.
* **Gestión de cambios:** El flujo de trabajo solo sube los archivos nuevos o que han sido modificados, lo que optimiza el uso de la API de GitHub.

## Configuración del Workflow

Para que el sistema de copias de seguridad funcione, debes importar el archivo `n8n backup.json` en tu instancia de n8n y configurarlo.

1.  **Importa el flujo de trabajo:** Importa el archivo `n8n backup.json` en tu instancia de n8n.
2.  **Configura las credenciales:** Asegúrate de que los nodos `n8n` y `GitHub` tengan las credenciales correctas.
3.  **Ajusta la configuración global:** Edita el nodo `Globals` en el flujo de trabajo secundario para establecer la configuración de tu repositorio de GitHub:
    * `repo.owner`: Tu nombre de usuario de GitHub.
    * `repo.name`: El nombre de tu repositorio.
    * `repo.path`: La carpeta donde se guardarán los backups (por ejemplo, `workflows/`).



## Cómo funciona

El sistema consta de dos flujos de trabajo de n8n:
* **Flujo de trabajo principal:** Este flujo se activa manualmente o por un temporizador. Recorre todos los flujos de trabajo activos en n8n y pasa la información de cada uno al sub-flujo de trabajo.
* **Sub-flujo de trabajo (`n8n backup`):** Este flujo recibe los datos de un único workflow, verifica si tiene una etiqueta para decidir la ruta del archivo y luego compara el estado actual con el backup en GitHub. Si el archivo es nuevo o diferente, se sube la versión actualizada al repositorio.

## Cómo usarlo

Para que tu flujo de trabajo se respalde correctamente:

* **Asigna una etiqueta:** Asegúrate de que cada flujo de trabajo que desees respaldar tenga al menos una etiqueta asignada en n8n. Por ejemplo, `modulo-1` o `automatización`.

El flujo de trabajo principal de respaldo se encargará de todo lo demás de forma automática.

## Revisa el historial de versiones

Para ver el historial de cambios de un flujo de trabajo específico:
1.  En este repositorio, navega a la carpeta donde se guardan los archivos (por ejemplo, `workflows/`).
2.  Selecciona el archivo `.json` del workflow que deseas ver.
3.  Haz clic en el botón **"History"** (Historial) en la parte superior derecha de la vista del archivo.
4.  Esto te mostrará una lista de todos los cambios guardados. Puedes hacer clic en cada "commit" para ver exactamente qué se ha añadido o eliminado.

