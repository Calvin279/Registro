<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro de Asistencia</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css"></link>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
</head>
<body class="bg-gray-100 font-roboto">
    <div class="container mx-auto p-4">
        <h1 class="text-3xl font-bold text-center mb-6">Registro de Asistencia</h1>
        
        <div class="bg-white p-6 rounded-lg shadow-lg mb-6">
            <h2 class="text-2xl font-bold mb-4">Registrar Entrada/Salida</h2>
            <form id="registroForm" class="space-y-4">
                <div>
                    <label for="nombre" class="block text-sm font-medium text-gray-700">Nombre</label>
                    <input type="text" id="nombre" name="nombre" class="mt-1 block w-full p-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                </div>
                <div>
                    <label for="rango" class="block text-sm font-medium text-gray-700">Rango</label>
                    <input type="text" id="rango" name="rango" class="mt-1 block w-full p-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                </div>
                <div>
                    <label for="horaEntrada" class="block text-sm font-medium text-gray-700">Hora de Entrada</label>
                    <input type="time" id="horaEntrada" name="horaEntrada" class="mt-1 block w-full p-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                </div>
                <div>
                    <label for="horaSalida" class="block text-sm font-medium text-gray-700">Hora de Salida</label>
                    <input type="time" id="horaSalida" name="horaSalida" class="mt-1 block w-full p-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                </div>
                <button type="button" onclick="registrar()" class="w-full bg-indigo-600 text-white p-2 rounded-md shadow-md hover:bg-indigo-700">Registrar</button>
            </form>
        </div>

        <div class="bg-white p-6 rounded-lg shadow-lg">
            <h2 class="text-2xl font-bold mb-4">Buscar Registros</h2>
            <form id="buscarForm" class="space-y-4">
                <div>
                    <label for="buscarNombre" class="block text-sm font-medium text-gray-700">Nombre</label>
                    <input type="text" id="buscarNombre" name="buscarNombre" class="mt-1 block w-full p-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
                </div>
                <button type="button" onclick="buscar()" class="w-full bg-indigo-600 text-white p-2 rounded-md shadow-md hover:bg-indigo-700">Buscar</button>
            </form>
        </div>

        <div id="resultados" class="mt-6">
            <!-- Resultados de la búsqueda aparecerán aquí -->
        </div>
    </div>

    <script>
        const registros = [];

        function registrar() {
            const nombre = document.getElementById('nombre').value;
            const rango = document.getElementById('rango').value;
            const horaEntrada = document.getElementById('horaEntrada').value;
            const horaSalida = document.getElementById('horaSalida').value;

            if (nombre && rango && horaEntrada && horaSalida) {
                registros.push({ nombre, rango, horaEntrada, horaSalida });
                alert('Registro añadido exitosamente');
                document.getElementById('registroForm').reset();
            } else {
                alert('Por favor, complete todos los campos');
            }
        }

        function buscar() {
            const buscarNombre = document.getElementById('buscarNombre').value;
            const resultados = document.getElementById('resultados');
            resultados.innerHTML = '';

            const registrosFiltrados = registros.filter(registro => registro.nombre.toLowerCase().includes(buscarNombre.toLowerCase()));

            if (registrosFiltrados.length > 0) {
                registrosFiltrados.forEach(registro => {
                    const horasTrabajadas = calcularHoras(registro.horaEntrada, registro.horaSalida);
                    const div = document.createElement('div');
                    div.classList.add('bg-white', 'p-4', 'rounded-lg', 'shadow-lg', 'mb-4');
                    div.innerHTML = `
                        <p><strong>Nombre:</strong> ${registro.nombre}</p>
                        <p><strong>Rango:</strong> ${registro.rango}</p>
                        <p><strong>Hora de Entrada:</strong> ${registro.horaEntrada}</p>
                        <p><strong>Hora de Salida:</strong> ${registro.horaSalida}</p>
                        <p><strong>Horas Trabajadas:</strong> ${horasTrabajadas.horas} horas y ${horasTrabajadas.minutos} minutos</p>
                    `;
                    resultados.appendChild(div);
                });
            } else {
                resultados.innerHTML = '<p class="text-center text-gray-500">No se encontraron registros</p>';
            }
        }

        function calcularHoras(horaEntrada, horaSalida) {
            const [horaE, minutoE] = horaEntrada.split(':').map(Number);
            const [horaS, minutoS] = horaSalida.split(':').map(Number);

            let horas = horaS - horaE;
            let minutos = minutoS - minutoE;

            if (minutos < 0) {
                minutos += 60;
                horas -= 1;
            }

            return { horas, minutos };
