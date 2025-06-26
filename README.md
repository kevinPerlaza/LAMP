<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Guía Interactiva de Instalación LAMP</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #111827; /* gray-900 */
            color: green; /* gray-100 */
        }
        .step-card {
            background-color: #1f2937; /* gray-800 */
            border: 1px solid #374151; /* gray-700 */
            border-radius: 0.75rem;
            transition: all 0.3s ease;
        }
        .step-card.completed {
            background-color: #374151; /* gray-700 */
            border-left: 4px solid #34d399; /* emerald-400 */
        }
        .step-card.completed h3 {
            text-decoration: line-through;
            color: #9ca3af; /* gray-400 */
        }
        pre {
            background-color: #0d1117;
            color: #c9d1d9;
            padding: 1rem;
            border-radius: 0.5rem;
            overflow-x: auto;
            position: relative;
        }
        .copy-btn {
            position: absolute;
            top: 0.5rem;
            right: 0.5rem;
            background-color: #374151; /* gray-700 */
            color: #e5e7eb; /* gray-200 */
            border: none;
            padding: 0.25rem 0.5rem;
            border-radius: 0.375rem;
            cursor: pointer;
            font-size: 0.875rem;
            transition: background-color 0.2s;
        }
        .copy-btn:hover {
            background-color: #4b5563; /* gray-600 */
        }
        .copy-btn-icon {
            width: 1rem;
            height: 1rem;
            display: inline-block;
            vertical-align: middle;
        }
        .tooltip {
            position: absolute;
            background-color: #10B981;
            color: white;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 0.8rem;
            top: -30px;
            right: 0;
            opacity: 0;
            transition: opacity 0.3s;
            pointer-events: none;
        }
    </style>
</head>
<body class="p-4 sm:p-6 md:p-8">

    <div class="max-w-4xl mx-auto">
        <header class="text-center mb-8">
            <h1 class="text-3xl sm:text-4xl font-bold text-white mb-2">Guía Interactiva de Instalación LAMP</h1>
            <p class="text-lg text-gray-400">Un tutorial paso a paso basado en tu PDF "Clase-13".</p>
        </header>

        <main class="space-y-6">
            <!-- Introducción -->
            <div class="step-card p-6">
                <h2 class="text-2xl font-semibold text-emerald-400 mb-4">¿Qué es LAMP?</h2>
                <p class="text-gray-300">LAMP es un acrónimo para un conjunto de software de código abierto muy popular para crear sitios y aplicaciones web. Representa:</p>
                <ul class="list-disc list-inside mt-4 space-y-2 text-lg">
                    <li><span class="font-bold text-white">L</span>inux: El sistema operativo.</li>
                    <li><span class="font-bold text-white">A</span>pache: El servidor web.</li>
                    <li><span class="font-bold text-white">M</span>ySQL: El sistema de gestión de bases de datos.</li>
                    <li><span class="font-bold text-white">P</span>HP: El lenguaje de programación del lado del servidor.</li>
                </ul>
            </div>

            <!-- Pasos de Instalación -->
            <div id="steps-container">
                <!-- Los pasos se insertarán aquí dinámicamente -->
            </div>
        </main>

        <footer class="text-center mt-12 text-gray-500">
            <p>Creado a partir del documento "Clase-13.pdf".</p>
        </footer>
    </div>

    <script>
        const stepsData = [
            {
                title: "Paso 1: Actualizar el Sistema",
                description: "Antes de instalar cualquier paquete nuevo, es una buena práctica actualizar la lista de paquetes y actualizar los paquetes existentes en tu sistema.",
                commands: [
                    "sudo apt update",
                    "sudo apt upgrade"
                ]
            },
            {
                title: "Paso 2: Instalar y Verificar Apache",
                description: "Instala el servidor web Apache. Es el servidor HTTP más utilizado en el mundo.",
                commands: [
                    "sudo apt install apache2"
                ],
                verification: "Después de la instalación, verifica que Apache esté funcionando. Primero, comprueba el estado del servicio. Deberías ver 'active (running)'. Luego, si tienes un firewall (como UFW) activado, asegúrate de permitir el tráfico de Apache. Finalmente, abre la dirección IP de tu servidor en un navegador web para ver la página predeterminada.",
                commands_verification: [
                    "sudo systemctl status apache2",
                    "sudo ufw allow 'Apache'"
                ]
            },
            {
                title: "Paso 3: Instalar MySQL",
                description: "A continuación, instala MySQL, un popular sistema de gestión de bases de datos relacionales.",
                commands: [
                    "sudo apt install mysql-server"
                ],
                verification: "Una vez instalado, puedes acceder a la consola de MySQL con el siguiente comando para confirmar que funciona. Escribe 'exit' para salir.",
                commands_verification: [
                    "sudo mysql"
                ]
            },
            {
                title: "Paso 4: Instalar PHP",
                description: "Instala PHP y los módulos necesarios para que funcione con Apache y MySQL.",
                commands: [
                    "sudo apt install php libapache2-mod-php php-mysql"
                ],
                verification: "Verifica la versión de PHP instalada.",
                commands_verification: [
                    "php -v"
                ]
            },
            {
                title: "Paso 5: Configurar el Directorio del Proyecto",
                description: "Crea un directorio para tu proyecto dentro de `/var/www/` y asigna los permisos adecuados. Reemplaza `nombredominio` con tu nombre de dominio o proyecto.",
                commands: [
                    "sudo mkdir /var/www/nombredominio",
                    "sudo chown -R $USER:$USER /var/www/nombredominio"
                ]
            },
            {
                title: "Paso 6: Crear y Configurar el Virtual Host",
                description: "Crea un archivo de configuración de host virtual para tu dominio. Esto le dice a Apache cómo servir tu sitio.",
                commands: [
                    "sudo nano /etc/apache2/sites-available/nombredominio.conf"
                ],
                extraContent: `
                    <p class="text-gray-400 mb-2">Dentro del editor \`nano\`, pega la siguiente configuración. Esto le indica a Apache dónde encontrar los archivos de tu sitio.</p>
                    <div class="relative">
                        <pre><code>&lt;VirtualHost *:80&gt;
    ServerName nombredominio
    ServerAlias www.nombredominio.ciber
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/nombredominio
    ErrorLog \${APACHE_LOG_DIR}/error.log
    CustomLog \${APACHE_LOG_DIR}/access.log combined
&lt;/VirtualHost&gt;</code></pre>
                        <button class="copy-btn" title="Copiar al portapapeles">
                            <svg class="copy-btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg>
                        </button>
                    </div>
                `
            },
            {
                title: "Paso 7: Habilitar el Nuevo Sitio",
                description: "Habilita tu nuevo archivo de host virtual y deshabilita el sitio predeterminado. Luego, prueba la configuración de Apache y recarga el servicio para aplicar los cambios.",
                commands: [
                    "sudo a2ensite nombredominio",
                    "sudo a2dissite 000-default",
                    "sudo apache2ctl configtest",
                    "sudo systemctl reload apache2"
                ],
                verification: "Si `configtest` devuelve `Syntax OK`, todo está correcto."
            },
            {
                title: "Paso 8: Crear una Página de Prueba HTML",
                description: "Crea un archivo `index.html` simple en el directorio de tu proyecto para confirmar que el host virtual funciona.",
                commands: [
                    "sudo nano /var/www/nombredominio/index.html"
                ],
                extraContent: `
                    <p class="text-gray-400 mb-2">Dentro de \`nano\`, escribe el siguiente HTML y guarda el archivo:</p>
                    <div class="relative">
                        <pre><code>&lt;h1&gt;Página funcionando&lt;/h1&gt;</code></pre>
                        <button class="copy-btn" title="Copiar al portapapeles">
                            <svg class="copy-btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg>
                        </button>
                    </div>
                    <p class="text-gray-400 mt-4">Ahora, al visitar la IP de tu servidor, deberías ver este mensaje.</p>
                `
            },
            {
                title: "Paso 9: Priorizar PHP y Probar el Procesamiento",
                description: "Modifica la configuración de Apache para que busque archivos `index.php` antes que `index.html`.",
                commands: [
                    "sudo nano /etc/apache2/mods-enabled/dir.conf"
                ],
                 extraContent: `
                    <p class="text-gray-400 mb-2">Asegúrate de que \`index.php\` esté primero en la lista \`DirectoryIndex\`.</p>
                    <div class="relative">
                        <pre><code>&lt;IfModule mod_dir.c&gt;
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
&lt;/IfModule&gt;</code></pre>
                         <button class="copy-btn" title="Copiar al portapapeles">
                            <svg class="copy-btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg>
                        </button>
                    </div>
                    <p class="text-gray-400 my-4">Recarga Apache para aplicar los cambios:</p>
                    <div class="relative"><pre><code>sudo systemctl reload apache2</code></pre><button class="copy-btn" title="Copiar al portapapeles"><svg class="copy-btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg></button></div>
                    <p class="text-gray-400 my-4">Ahora, crea un archivo PHP para probar que se procesa correctamente.</p>
                    <div class="relative"><pre><code>sudo nano /var/www/nombredominio/info.php</code></pre><button class="copy-btn" title="Copiar al portapapeles"><svg class="copy-btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg></button></div>
                    <p class="text-gray-400 my-4">Dentro de \`nano\`, añade el siguiente código PHP:</p>
                    <div class="relative"><pre><code>&lt;?php
phpinfo();</code></pre><button class="copy-btn" title="Copiar al portapapeles"><svg class="copy-btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg></button></div>
                    <p class="text-gray-400 mt-4">Visita <code>http://ip-servidor/info.php</code>. Deberías ver una página con toda la información sobre tu instalación de PHP.</p>
                `
            },
            {
                title: "Paso 10: Limpieza",
                description: "Es una buena práctica de seguridad eliminar el archivo `info.php` después de verificar que PHP funciona, ya que contiene información sensible sobre tu servidor.",
                commands: [
                    "sudo rm /var/www/nombredominio/info.php"
                ],
                verification: "¡Felicidades! Has completado la instalación y configuración de tu entorno LAMP."
            }
        ];

        const stepsContainer = document.getElementById('steps-container');

        stepsData.forEach((step, index) => {
            const stepId = `step-${index}`;
            const card = document.createElement('div');
            card.className = 'step-card p-6';
            card.id = stepId;

            let commandsHTML = '';
            if (step.commands) {
                commandsHTML += step.commands.map(cmd => `
                    <div class="relative mt-2">
                        <pre><code>${cmd}</code></pre>
                        <button class="copy-btn" title="Copiar al portapapeles">
                            <svg class="copy-btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg>
                        </button>
                    </div>
                `).join('');
            }

            let verificationHTML = '';
            if (step.verification) {
                verificationHTML += `<div class="mt-4 pt-4 border-t border-gray-700"><p class="text-gray-300">${step.verification}</p></div>`;
                if(step.commands_verification) {
                    verificationHTML += step.commands_verification.map(cmd => `
                    <div class="relative mt-2">
                        <pre><code>${cmd}</code></pre>
                        <button class="copy-btn" title="Copiar al portapapeles">
                           <svg class="copy-btn-icon" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"></path></svg>
                        </button>
                    </div>
                    `).join('');
                }
            }
            
            let extraContentHTML = step.extraContent || '';

            card.innerHTML = `
                <div class="flex items-start">
                    <div class="flex-grow">
                        <h3 class="text-xl font-semibold text-white">${step.title}</h3>
                        <p class="text-gray-400 mt-2">${step.description}</p>
                    </div>
                    <div class="ml-4 flex-shrink-0">
                        <input type="checkbox" class="h-6 w-6 rounded border-gray-600 bg-gray-900 text-emerald-500 focus:ring-emerald-500 cursor-pointer" onchange="toggleStepCompletion('${stepId}')" title="Marcar como completado">
                    </div>
                </div>
                <div class="mt-4">
                    ${commandsHTML}
                    ${extraContentHTML}
                    ${verificationHTML}
                </div>
            `;
            stepsContainer.appendChild(card);
        });

        function toggleStepCompletion(stepId) {
            document.getElementById(stepId).classList.toggle('completed');
        }

        document.body.addEventListener('click', function(event) {
            const button = event.target.closest('.copy-btn');
            if (button) {
                const pre = button.closest('.relative').querySelector('pre');
                const code = pre.querySelector('code');
                const textToCopy = code.innerText;

                // Fallback for document.execCommand
                const textArea = document.createElement('textarea');
                textArea.value = textToCopy;
                document.body.appendChild(textArea);
                textArea.select();
                try {
                    document.execCommand('copy');
                    showTooltip(button, 'Copiado!');
                } catch (err) {
                    showTooltip(button, 'Error al copiar');
                }
                document.body.removeChild(textArea);
            }
        });

        function showTooltip(button, message) {
            const tooltip = document.createElement('div');
            tooltip.className = 'tooltip';
            tooltip.textContent = message;
            button.appendChild(tooltip);

            requestAnimationFrame(() => {
                tooltip.style.opacity = 1;
            });

            setTimeout(() => {
                tooltip.style.opacity = 0;
                setTimeout(() => {
                    button.removeChild(tooltip);
                }, 300);
            }, 2000);
        }

    </script>
</body>
</html>


