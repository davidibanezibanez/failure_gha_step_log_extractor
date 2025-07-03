# Para ejecutar

1. Crear entorno, activarlo e instalar requirements.txt.
2. Crear archivo .env con variable GITHUB_TOKEN.
3. Configurar OWNER y REPO en el notebook.

# Flujo del programa

1. El programa analiza un repo.
2. Encuentra los workflow runs fallidos (conclusion == 'failure').
3. Por cada workflow run fallido, guarda workflow_run.json, jobs.json y archivo .yml.
4. Luego por cada job que falló en ese workflow guarda el json con la metadata del job específico y su archivo de log en .txt (Jobs con "conclusion == 'failure'").
5. Luego, por cada job que falló, guarda los logs de los steps fallidos de ese job.

# Información adicional

## Seguimiento de los steps en json y log

Los steps no poseen id en la metadata json, el identificador más próximo es name, sin embargo algunos step no poseen name definido y GitHub les asigna un name en la metadata de acuerdo al comando que ejecutan. En cualquier caso, tengan nombre definido o no en el yaml, en el log el identificar que aparecerá al inicio del step es un nombre asignado de acuerdo al comando que ejecutan. 

Por ejemplo para el siguiente step:

```
{
    "name": "Create Pull Request",
    "status": "completed",
    "conclusion": "failure",
    "number": 6,
    "started_at": "2025-07-03T00:15:21Z",
    "completed_at": "2025-07-03T00:15:29Z"
},
```

```
-   name: Create Pull Request
    shell: bash
    run: node scripts/automated-update-workflow.js
    env:
        GITHUB_TOKEN: ${{ secrets.GH_TOKEN_PULL_REQUESTS }}
        BRANCH_NAME: fonts-data
        SCRIPT: scripts/update-google-fonts.js
        PR_TITLE: Update font data
        PR_BODY: This auto-generated PR updates font data with latest available
```

Su identificador en la primera línea del log es:

```
2025-07-03T00:15:21.1416628Z ##[group]Run node scripts/automated-update-workflow.js
```

Para este caso, si en el archivo yaml no tuviera un name definido, entonces en la metadata json su name simplemente sería "Run node scripts/automated-update-workflow.js".

## ¿Existe un JSON que contenga workflow run y jobs, ambos compilados en un solo JSON de manera original desde la API?

No existe un JSON único original desde la API de GitHub, que contenga tanto el workflow_run como los jobs juntos.

## ¿Con este programa tenemos todo el historial de workflows fallidos del repo?

No, debido a que es posible extraer hasta un máximo de 1000 workflow runs por repo con API normal de GitHub. Además el programa tiene un límite de 90 días pasados desde la ejecución, debido a que GitHub no almacena logs de más de 90 días.

Fuente máximo 1000 workflow runs: https://docs.github.com/en/rest/actions/workflow-runs?apiVersion=2022-11-28.
