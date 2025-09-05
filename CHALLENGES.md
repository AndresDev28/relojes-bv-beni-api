## Desafío de Configuración del Backend: La Caza del Bug "Fantasma" de Autenticación

**Problema:** Tras configurar el plugin de Users & Permissions en Strapi, el endpoint de login (`/api/auth/local`) fallaba consistentemente con errores `403 Forbidden` y `500 Internal Server Error`, a pesar de que la configuración de roles y permisos parecía ser la correcta.

**Investigación:** Este fue un bug de entorno muy persistente. El proceso de depuración incluyó:
1.  Verificación exhaustiva de los permisos para los roles `Public` y `Authenticated`.
2.  Análisis de la configuración de confirmación de email y ajustes avanzados del plugin.
3.  Múltiples pruebas con usuarios nuevos para descartar bloqueos a nivel de usuario.
4.  Reinstalación del plugin `users-permissions` para intentar corregir una posible corrupción.

**Solución:**
La persistencia de los errores a pesar de las reconfiguraciones apuntaba a un estado corrupto en la instalación local de Strapi que no se solucionaba con métodos convencionales. La solución definitiva fue un **"reseteo de fábrica"** del entorno de desarrollo del backend:
1.  **Copia de seguridad** de la lógica de negocio (`src/api`).
2.  **Limpieza nuclear:** Eliminación de `node_modules`, `.tmp`, `build`, `dist` y `package-lock.json`.
3.  **Reinstalación limpia** de todas las dependencias con `npm install`.
4.  **Reconstrucción** del proyecto con `npm run build`.
5.  **Regeneración de las claves de seguridad** (`.env` y `config/server.ts`).

**Resultado:** Al arrancar desde un estado completamente limpio, el bug "fantasma" desapareció. El registro y el login comenzaron a funcionar como se esperaba. La lección clave fue aprender a identificar cuándo un problema ya no es de lógica, sino del propio entorno, y aplicar medidas drásticas pero efectivas para restaurar la estabilidad sin perder los avances que no estaban afectados por bug.