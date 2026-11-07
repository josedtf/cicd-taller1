# cicd taller 1
## Integrantes
José David Toro Franco
usuario de github: josedtf

## Workflow
https://github.com/josedtf/cicd-taller1/actions/runs/42380930225

## Respuestas

### 1. ¿Qué eventos configuré y por qué?
Configuré dos eventos independientes dentro de `on`:
- `push` a la rama `main`: ejecuta el workflow automáticamente cada vez que subo cambios, así verifico de inmediato que la automatización funciona con el código más reciente.
- `workflow_dispatch`: permite ejecutarlo manualmente desde la pestaña Actions (botón "Run workflow"), útil para repetir pruebas sin hacer un commit nuevo.

### 2. Sistema operativo del runner y relación con el shell
El runner `ubuntu-latest` es una máquina virtual con Ubuntu Linux que GitHub aloja y mantiene. Puede ejecutar mi workflow aunque mi computadora sea Windows o macOS porque los pasos `run` se ejecutan en esa máquina virtual en la nube, no en mi equipo; mi computadora solo sube el código. El runner determina el shell por defecto de los pasos `run` (Bash en Ubuntu/macOS, PowerShell en Windows), por eso fijé `shell: bash` en el paso del mensaje y usé `$(date)`, que funciona en Bash. La fecha mostrada es la del runner, no la de mi computadora.

### 3. Acciones del Marketplace
- `actions/cache`: guarda y restaura dependencias (por ejemplo, paquetes de pip o npm) entre ejecuciones para acelerar los workflows.
- `github/codeql-action`: ejecuta análisis estático de código (CodeQL) para detectar vulnerabilidades de seguridad.
- `actions/github-script`: permite ejecutar JavaScript con la API de GitHub dentro del workflow, por ejemplo para comentar en issues o pull requests automáticamente.

### 4. ¿Qué es un secreto?
Un secreto es un valor sensible (tokens, contraseñas, claves de API) que se guarda cifrado en el repositorio, fuera del código, en Settings > Secrets and variables > Actions > New repository secret. Se usa para que los workflows accedan a servicios externos sin exponer credenciales en el código ni en los logs. En un workflow se referencia con `${{ secrets.NOMBRE_DEL_SECRETO }}` o `${{ secrets.AWS_SECRET_ACCESS_KEY }}`, como se menciona en el ejemplo de AWS

### 5. Reflexión: ¿qué automatizaría?
- Ejecutar pruebas automáticas en cada push o pull request, para detectar errores de integración lo antes posible y no depender de revisiones manuales.
- Escaneos de seguridad en cada cambio. En una clase anterior ya trabajé con esto: en el taller de escaneo de seguridad ejecuté análisis estático (SAST), de dependencias (SCA) y de secretos sobre AltoroJ con GitHub Actions. Lo automatizaría en mis proyectos porque detectar una vulnerabilidad o una credencial expuesta al momento del cambio es mucho más barato que hacerlo cuando el código ya está en producción.
