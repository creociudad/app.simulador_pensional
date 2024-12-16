<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador de Aportes Pensionales</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 20px;
            background-color: #f9f9f9;
        }
        .calculator {
            max-width: 400px;
            margin: auto;
            padding: 20px;
            border: 1px solid #ccc;
            border-radius: 8px;
            background: #ffffff;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        .calculator h2 {
            text-align: center;
            color: #333;
        }
        .form-group {
            margin-bottom: 15px;
        }
        label {
            font-weight: bold;
        }
        select, input {
            width: 100%;
            padding: 8px;
            margin-top: 5px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        button {
            width: 100%;
            padding: 10px;
            background-color: #007bff;
            color: #ffffff;
            border: none;
            border-radius: 4px;
            font-size: 16px;
            cursor: pointer;
        }
        button:hover {
            background-color: #0056b3;
        }
        .result {
            margin-top: 20px;
            padding: 15px;
            border: 1px solid #ddd;
            background: #f7f7f7;
            border-radius: 4px;
        }
    </style>
</head>
<body>

<div class="calculator">
    <h2>Simulador de Aportes Pensionales</h2>
    <div class="form-group">
        <label for="tipoAportante">Elige tipo de aportante/cotizante:</label>
        <select id="tipoAportante">
            <option value="empleado">Empleado (Con Contrato Laboral)</option>
            <option value="independiente">Independiente</option>
            <option value="contratista">Independiente (Contratista por Prestación de Servicios)</option>
        </select>
    </div>
    <div class="form-group">
        <label for="smmlv">Seleccione el número de SMMLV:</label>
        <select id="smmlv">
            <option value="1">1 SMMLV</option>
            <option value="2">2 SMMLV</option>
            <option value="3">3 SMMLV</option>
            <option value="4">4 SMMLV</option>
            <option value="5">5 SMMLV</option>
        </select>
    </div>
    <button onclick="calcularAporte()">Calcular</button>
    <div class="result" id="result">
        Seleccione un valor y presione "Calcular" para ver los resultados.
    </div>
</div>

<script>
    const SMMLV_2024 = 1300000; // Salario mínimo para 2024
    const PORCENTAJE_APORTE = 0.16; // 16% de aporte a pensión
    const PORCENTAJE_IBC = 0.4; // 40% del ingreso base de cotización
    
    function calcularAporte() {
        // Obtener el tipo de aportante seleccionado
        const tipoAportante = document.getElementById("tipoAportante").value;

        // Obtener el número de SMMLV seleccionado
        const smmlvSeleccionado = document.getElementById("smmlv").value;

        // Calcular el salario total
        const salario = SMMLV_2024 * smmlvSeleccionado;

        // Calcular el IBC (40% del salario total)
        const ibc = salario * PORCENTAJE_IBC;

        // Inicializar variables para los aportes
        let aportePension = 0;
        let aporteEmpleador = 0;
        let aporteEmpleado = 0;

        // Mostrar los resultados
        const resultDiv = document.getElementById("result");

        if (tipoAportante === "contratista") {
            // Mostrar mensaje para contratistas
            resultDiv.innerHTML = `
                <p><strong>Tipo de Aportante:</strong> Independiente (Contratista por Prestación de Servicios)</p>
                <p>Está por definirse el cálculo de la cotización en la reforma laboral actualmente en curso. Por el momento estamos atentos a cómo avanza el debate para poder brindarte la información más precisa.</p>
            `;
        } else if (tipoAportante === "empleado") {
            // Cálculo para empleados
            aporteEmpleador = ibc * 0.12; // 12% del IBC
            aporteEmpleado = ibc * 0.04;  // 4% del IBC
            resultDiv.innerHTML = `
                <p><strong>Tipo de Aportante:</strong> Empleado (Con Contrato Laboral)</p>
                <p><strong>Ingreso/Salario (${smmlvSeleccionado} SMMLV):</strong> $${salario.toLocaleString()}</p>
                <p><strong>Ingreso Base de Cotización (IBC del 40%):</strong> $${ibc.toLocaleString()}</p>
                <p><strong>Aporte del Empleador (12% del IBC):</strong> $${aporteEmpleador.toLocaleString()}</p>
                <p><strong>Aporte del Empleado (Deducido en el pago de nómina) (4% del IBC):</strong> $${aporteEmpleado.toLocaleString()}</p>
            `;
        } else {
            // Cálculo para independientes
            aportePension = ibc * PORCENTAJE_APORTE; // 16% del IBC
            resultDiv.innerHTML = `
                <p><strong>Tipo de Aportante:</strong> Independiente</p>
                <p><strong>Valor del SMMLV (${smmlvSeleccionado} SMMLV):</strong> $${salario.toLocaleString()}</p>
                <p><strong>Ingreso Base de Cotización (IBC del 40%):</strong> $${ibc.toLocaleString()}</p>
                <p><strong>Aporte a Pensión (16% del IBC):</strong> $${aportePension.toLocaleString()}</p>
            `;
        }
    }
</script>

</body>
</html>
