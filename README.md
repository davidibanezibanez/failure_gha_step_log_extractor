# Para ejecutar:

1. Crear entorno, activarlo e instalar requirements.txt.
2. Crear archivo .env con variable GITHUB_TOKEN.
3. Configurar OWNER y REPO en el notebook.

# Flujo del programa:

1. El programa analiza un repo.
2. Encuentra todos los workflow runs fallidos (Gracias a la paginación (máximo 10 páginas de 100 workflow fallidos) todos los workflow runs con conclusion == 'failure' son incluidos).
3. Por cada workflow run fallido, trae un log .txt de ese workflow
4. Luego trae los logs por cada job que falló en ese workflow (El programa solo incluye jobs con "conclusion == 'failure'") (No descarga jobs exitosos ni omitidos).
5. Luego, por cada job que falló, trae los logs de todos los steps de ese job.
6. Además de los logs por step, extrae los mensajes de error de los steps que fallaron (Archivos separados con sufijo _errors.txt por cada step fallido).

# ¿Tenemos todos los niveles del workflow con nuestro programa?

Sí. Workflow run fallido, jobs fallidos, steps, mensajes de error.

# ¿Existe a nivel de job un parámetro para determinar si es failure o no?

Sí, existe el campo 'conclusion' a nivel de workflow, job y step. Pero para obtener 'conclusion' a nivel de step se debe llamar a otro endpoint de la API.

# Si un mismo workflow falla varias veces, ¿nuestro programa traerá todos los fallos que tuvo ese mismo workflow?

Sí. Cada workflow run tiene su propio ID independientemente de cuantas veces se ejecute el mismo workflow.

# ¿Tenemos absolutamente TODO el historial del repo con nuestro programa?

No absolutamente todo, pero todo lo posible con la API normal de GitHub (Máximo de 1000 workflow runs por repo con API normal de GitHub).
