Instalación de MAMP
1. Descargamos MAMP desde https://www.mamp.info.
2. 3. Instalamos MAMP en el ordenador.
Abrimos MAMP y arrancamos los servidores Apache y MySQL.
4. Verificamos que funciona abriendo:
http://localhost:8888
(debería mostrar la página de inicio de MAMP).
Instalación de Moodle local
1. Descargamos Moodle desde https://download.moodle.org.
2. Descomprimimos el paquete en la carpeta:
/Applications/MAMP/htdocs/ (en macOS)
o C:\MAMP\htdocs\ (en Windows).
3. Renombramos la carpeta, por ejemplo:
moodle500
4. En el navegador abrimos:
http://localhost:8888/moodle500
5. Seguimos el instalador paso a paso:
o Elegimos idioma
o Verificamos requisitos
o Configuramos la base de datos (MySQL desde MAMP)
o Creamos el usuario administrador
6. Cuando termina la instalación, ya tenemos Moodle funcionando localmente en:
http://localhost:8888/moodle500/
Instalación de MoodleApp (con Ionic)
1. Instalamos Node.js (si no lo tenemos).
2. Instalamos Ionic CLI globalmente:
3. npm install -g @ionic/cli
4. Clonamos o descargamos el código de MoodleApp oficial:
5. git clone https://github.com/moodlehq/moodleapp.git
6. cd moodleapp
7. npm install
8. Ejecutamos la app:
9. ionic serve
Se abrirá en http://localhost:8100
Problema encontrado
• La app no podía conectarse al Moodle local.
• Mostraba un bloqueo CORS (Cross-Origin Resource Sharing).
Solución del CORS
1. En el archivo config.php de Moodle (dentro de moodle500/config.php) añadimos:
2. $CFG->wwwroot = 'http://localhost:8888/moodle500';
3. $CFG->allowthemechangeonurl = true;
4. $CFG->alloworigin = ['http://localhost:8100'];
5. Limpiamos la caché de Moodle desde Administración del sitio → Desarrollo →
Vaciar todas las cachés.
6. Ahora la app puede conectarse correctamente al Moodle local.
Conexión automática (sin escribir la URL
manualmente)
Para que la app use el Moodle local automáticamente al abrirla
a) Configuramos un proxy en Ionic
Archivo: ionic.config.json
{
"name": "moodlemobile",
"integrations": { "cordova": {} },
"type": "angular",
"hooks": {
"build:before": "./scripts/copy-assets.js",
"serve:before": "./scripts/copy-assets.js"
},
"proxies": [
{
"path": "/moodle",
"proxyUrl": "http://localhost:8888/moodle500"
}
]
}
b) Añadimos la URL por defecto
Archivo: src/assets/env.json
"siteurl": "http://localhost:8100/moodle/"
Resultado final
Moodle funcionando localmente en http://localhost:8888/moodle500/
MoodleApp corriendo en http://localhost:8100
La app se conecta automáticamente al Moodle local
Sin escribir la URL manualmente
Sin errores CORS