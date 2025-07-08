# painel-de-aeroporto-
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Painel de Aeroporto</title>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@600;700&display=swap" rel="stylesheet">
    <style>
        body {
            background: linear-gradient(135deg, #e0e7ef 0%, #c9d6ff 100%);
            font-family: 'Montserrat', Arial, sans-serif;
            margin: 0;
            padding: 0;
            min-height: 100vh;
        }
        .animation {
            width: 100vw;
            height: 100vh;
            background: linear-gradient(135deg, #232526 0%, #414345 100%);
            color: #fff;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-size: 2.5em;
            letter-spacing: 2px;
            position: fixed;
            top: 0; left: 0;
            z-index: 10;
            animation: fadeOut 1.5s 2s forwards;
        }
        .plane {
            font-size: 3em;
            animation: planeMove 2s linear infinite;
            margin-bottom: 20px;
        }
        @keyframes planeMove {
            0% { transform: translateX(-30px) rotate(-10deg);}
            50% { transform: translateX(30px) rotate(10deg);}
            100% { transform: translateX(-30px) rotate(-10deg);}
        }
        @keyframes fadeOut {
            to { opacity: 0; visibility: hidden; }
        }
        .panel-container {
            max-width: 900px;
            margin: 60px auto 0 auto;
            background: #fff;
            border-radius: 18px;
            box-shadow: 0 8px 32px #0002;
            padding: 40px 32px 32px 32px;
            position: relative;
        }
        h1 {
            text-align: center;
            margin-bottom: 32px;
            color: #1a2533;
            font-size: 2.5em;
            letter-spacing: 1px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 1.15em;
            background: #f7fafd;
            border-radius: 10px;
            overflow: hidden;
        }
        th, td {
            padding: 14px 10px;
            border-bottom: 1px solid #e0e0e0;
            text-align: center;
        }
        th {
            background: #1a2533;
            color: #fff;
            font-weight: 700;
        }
        tr:last-child td {
            border-bottom: none;
        }
        .add-btn {
            margin: 18px 0 24px 0;
            padding: 10px 28px;
            background: linear-gradient(90deg, #1a2533 60%, #3b4b5b 100%);
            color: #fff;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.1em;
            font-weight: 600;
            transition: background 0.3s, transform 0.2s;
            box-shadow: 0 2px 8px #0001;
        }
        .add-btn:hover {
            background: linear-gradient(90deg, #3b4b5b 60%, #1a2533 100%);
            transform: scale(1.04);
        }
        .remove-btn {
            padding: 6px 18px;
            background: #e74c3c;
            color: #fff;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 1em;
            font-weight: 600;
            transition: background 0.2s, transform 0.2s;
        }
        .remove-btn:hover {
            background: #c0392b;
            transform: scale(1.07);
        }
        @media (max-width: 700px) {
            .panel-container {
                padding: 18px 4px;
            }
            table, th, td {
                font-size: 0.95em;
            }
        }
    </style>
</head>
<body>
    <div class="animation" id="animation">
        <span class="plane">✈️</span>
        <span style="margin-top: 18px; font-size: 0.7em; font-weight: 700; letter-spacing: 1px;">
            central de Informações de voos         </span>
    </div>
    <div class="panel-container" style="display:none;" id="panel">
        <h1>central de Informações de voos</h1>
        <button class="add-btn" onclick="addFlight()">Adicionar Voo</button>
        <table>
            <thead>
                <tr>
                    <th>Voo</th>
                    <th>Destino</th>
                    <th>Horário</th>
                    <th>Status</th>
                    <th>Ações</th>
                </tr>
            </thead>
            <tbody id="flight-table">
                <!-- Linhas dos voos aqui -->
            </tbody>
        </table>
    </div>
    <script>
        // Lista duplamente encadeada em JS
        class Node {
            constructor(data) {
                this.data = data;
                this.prev = null;
                this.next = null;
            }
        }
        class DoublyLinkedList {
            constructor() {
                this.head = null;
                this.tail = null;
            }
            append(data) {
                const node = new Node(data);
                if (!this.head) {
                    this.head = this.tail = node;
                } else {
                    node.prev = this.tail;
                    this.tail.next = node;
                    this.tail = node;
                }
            }
            remove(node) {
                if (node.prev) node.prev.next = node.next;
                else this.head = node.next;
                if (node.next) node.next.prev = node.prev;
                else this.tail = node.prev;
            }
            toArray() {
                let arr = [];
                let curr = this.head;
                while (curr) {
                    arr.push(curr);
                    curr = curr.next;
                }
                return arr;
            }
        }

        // Dados iniciais
        const flights = new DoublyLinkedList();
        flights.append({ voo: "JJ1234", destino: "São Paulo", horario: "08:30", status: "No horário" });
        flights.append({ voo: "LA5678", destino: "Rio de Janeiro", horario: "09:15", status: "Atrasado" });
        flights.append({ voo: "AZ4321", destino: "Brasília", horario: "10:00", status: "Embarque" });

        function renderFlights() {
            const tbody = document.getElementById('flight-table');
            tbody.innerHTML = '';
            flights.toArray().forEach(node => {
                const { voo, destino, horario, status } = node.data;
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td>${voo}</td>
                    <td>${destino}</td>
                    <td>${horario}</td>
                    <td>${status}</td>
                    <td>
                        <button class="remove-btn" onclick="removeFlight('${voo}')">Remover</button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function addFlight() {
            const voo = prompt("Número do voo:");
            const destino = prompt("Destino:");
            const horario = prompt("Horário (hh:mm):");
            const status = prompt("Status:");
            if (voo && destino && horario && status) {
                flights.append({ voo, destino, horario, status });
                renderFlights();
            }
        }

        function removeFlight(voo) {
            let curr = flights.head;
            while (curr) {
                if (curr.data.voo === voo) {
                    flights.remove(curr);
                    renderFlights();
                    break;
                }
                curr = curr.next;
            }
        }

        // Remover efeito de digitação e texto
        window.onload = function() {
            setTimeout(() => {
                document.getElementById('animation').style.display = 'none';
                document.getElementById('panel').style.display = 'block';
                renderFlights();
            }, 2200);
        }
    </script>
</body>
</html>
