#### Frontend

 - Código em único arquivo html.

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Painel Admin - Supermercado Delivery</title>
    <style>
        :root {
            --primary: #2563eb;
            --success: #16a34a;
            --danger: #dc2626;
            --dark: #1e293b;
            --light: #f8fafc;
        }
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', sans-serif; }
        body { background-color: var(--light); color: var(--dark); padding: 20px; }
        header { margin-bottom: 30px; text-align: center; }
        
        .container { max-width: 1200px; margin: 0 auto; display: grid; grid-template-columns: 1fr; gap: 20px; }
        @media(min-width: 768px) { .container { grid-template-columns: 1fr 2fr; } }
        
        .card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.1); }
        h2 { margin-bottom: 15px; font-size: 1.2rem; color: var(--primary); text-transform: uppercase; }
        
        form { display: flex; flex-direction: column; gap: 10px; }
        input { padding: 10px; border: 1px solid #cbd5e1; border-radius: 4px; font-size: 1rem; }
        button { padding: 10px; border: none; border-radius: 4px; background: var(--primary); color: white; cursor: pointer; font-weight: bold; }
        button:hover { opacity: 0.9; }
        
        .grid-pedidos { display: grid; grid-template-columns: 1fr; gap: 15px; }
        @media(min-width: 992px) { .grid-pedidos { grid-template-columns: 1fr 1fr; } }
        
        .pedido-item { background: #f1f5f9; padding: 15px; border-radius: 6px; border-left: 5px solid var(--primary); display: flex; flex-direction: column; gap: 8px; }
        .pedido-actions { display: flex; gap: 8px; margin-top: 10px; }
        .select-status { padding: 5px; border-radius: 4px; border: 1px solid #cbd5e1; }
        .btn-del { background: var(--danger); }
    </style>
</head>
<body>

    <header>
        <h1>Painel de Controle de Entregas</h1>
        <p>Monitoramento e Gestão de Pedidos em Tempo Real</p>
    </header>

    <div class="container">
        <div class="card">
            <h2>Novo Pedido</h2>
            <form id="form-pedido">
                <input type="text" id="cliente" placeholder="Nome do Cliente" required>
                <input type="text" id="endereco" placeholder="Endereço de Entrega" required>
                <input type="number" id="total" step="0.01" placeholder="Valor Total (R$)" required>
                <button type="submit">Cadastrar Pedido</button>
            </form>
        </div>

        <div class="card">
            <h2>Pedidos Ativos</h2>
            <div id="lista-pedidos" class="grid-pedidos">
                </div>
        </div>
    </div>

    <script>
        const API_URL = 'http://127.0.0.1:8000/pedidos';

        // Carregar pedidos ao iniciar
        document.addEventListener('DOMContentLoaded', fetchPedidos);

        // CREATE
        document.getElementById('form-pedido').addEventListener('submit', async (e) => {
            e.preventDefault();
            const novoPedido = {
                cliente_nome: document.getElementById('cliente').value,
                endereco_entrega: document.getElementById('endereco').value,
                valor_total: parseFloat(document.getElementById('total').value)
            };

            await fetch(API_URL, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(novoPedido)
            });

            document.getElementById('form-pedido').reset();
            fetchPedidos();
        });

        // READ
        async function fetchPedidos() {
            const res = await fetch(API_URL);
            const pedidos = await res.json();
            const container = document.getElementById('lista-pedidos');
            container.innerHTML = '';

            pedidos.forEach(pedido => {
                const el = document.createElement('div');
                el.className = 'pedido-item';
                el.innerHTML = `
                    <div><strong>ID:</strong> #${pedido.id}</div>
                    <div><strong>Cliente:</strong> ${pedido.cliente_nome}</div>
                    <div><strong>Destino:</strong> ${pedido.endereco_entrega}</div>
                    <div><strong>Total:</strong> R$ ${pedido.valor_total.toFixed(2)}</div>
                    <div><strong>Status:</strong> <span style="font-weight:bold">${pedido.status}</span></div>
                    
                    <div class="pedido-actions">
                        <select class="select-status" onchange="updateStatus(${pedido.id}, this.value)">
                            <option value="">Alterar Status...</option>
                            <option value="Pendente" ${pedido.status === 'Pendente' ? 'selected' : ''}>Pendente</option>
                            <option value="Em Rota" ${pedido.status === 'Em Rota' ? 'selected' : ''}>Em Rota</option>
                            <option value="Entregue" ${pedido.status === 'Entregue' ? 'selected' : ''}>Entregue</option>
                            <option value="Cancelado" ${pedido.status === 'Cancelado' ? 'selected' : ''}>Cancelado</option>
                        </select>
                        <button class="btn-del" onclick="deletePedido(${pedido.id})">Excluir</button>
                    </div>
                `;
                container.appendChild(el);
            });
        }

        // UPDATE
        async function updateStatus(id, novoStatus) {
            if(!novoStatus) return;
            await fetch(`${API_URL}/${id}`, {
                method: 'PUT',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ status: novoStatus })
            });
            fetchPedidos();
        }

        // DELETE
        async function deletePedido(id) {
            if(confirm("Deseja realmente remover este pedido?")) {
                await fetch(`${API_URL}/${id}`, { method: 'DELETE' });
                fetchPedidos();
            }
        }
    </script>
</body>
</html>
```
