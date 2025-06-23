# Para ejecutar

- Crear entorno, activarlo e instalar requirements.txt.

- Crear archivo .env con variable GITHUB_TOKEN.

- Configurar OWNER y REPO en el notebook.

- El programa crea los archivos en el directorio github_failure_logs/ (Un archivo .txt por cada failure encontrado).

- El programa incluye pausas para no sobrecargar la API.

# Flujo del programa y puntos importantes

- El programa busca workflows con estado "failure".

- Analiza los logs para identificar qué step exacto falló.

- Guarda en un archivo .txt los mensajes de error del step que falló por cada job failure.

- Cada archivo .txt incluye además los siguientes metadatos: repositorio, workflow, job, step, IDs y timestamp.

- El programa analiza hasta 10 workflow runs por repositorio (los 10 más recientes que encuentre).