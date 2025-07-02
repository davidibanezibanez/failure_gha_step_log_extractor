# Para ejecutar:

1. Crear entorno, activarlo e instalar requirements.txt.
2. Crear archivo .env con variable GITHUB_TOKEN.
3. Configurar OWNER y REPO en el notebook.

# Flujo del programa:

1. El programa analiza un repo.
2. Encuentra todos los workflow runs fallidos (Gracias a la paginación (máximo 10 páginas de 100 workflow fallidos) todos los workflow runs con conclusion == 'failure' son incluidos).
3. Por cada workflow run fallido, guarda workflow_run.json y jobs.json.
4. Luego por cada job que falló en ese workflow guarda el json con la metadata del job y su archivo de log en .txt (El programa solo incluye jobs con "conclusion == 'failure'", no descarga jobs exitosos ni omitidos).
5. Luego, por cada job que falló, guarda los logs de todos los steps de ese job.
6. Además de los logs por step, extrae los mensajes de error de los steps que fallaron (archivos separados con sufijo _errors.txt por cada step fallido).

# ¿Tenemos todos los niveles del workflow con nuestro programa?

Sí. Workflow run fallido, jobs fallidos, steps, mensajes de error.

# ¿Existe a nivel de job un parámetro para determinar si es failure o no?

Sí, existe el campo 'conclusion' a nivel de workflow, job y step.

# Si un mismo workflow falla varias veces, ¿nuestro programa traerá todos los fallos que tuvo ese mismo workflow?

Sí. Cada workflow run tiene su propio ID independientemente de cuantas veces se ejecute el mismo workflow.

# ¿Tenemos absolutamente TODO el historial del repo con nuestro programa?

No absolutamente todo, pero todo lo posible con la API normal de GitHub (Máximo de 1000 workflow runs por repo con API normal de GitHub).

Fuente: https://docs.github.com/en/rest/actions/workflow-runs?apiVersion=2022-11-28.

# ¿Existe un JSON que contenga workflow run y jobs, ambos compilados en un solo JSON de manera original desde la API?

No existe un JSON único original desde la API de GitHub, que contenga tanto el workflow_run como los jobs juntos.
