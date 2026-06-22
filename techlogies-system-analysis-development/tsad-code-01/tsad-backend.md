#### Backend code suggestion

```
from fastapi import FastAPI, HTTPException, Depends
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel
from typing import List, Optional
import sqlite3
```

```
app = FastAPI(title="Supermercado Delivery API")
```

```
# Permite chamadas do Front-end (CORS)
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

```
DB_NAME = "delivery.db"
```

##### Inicialização do Banco de Dados
```
def init_db():
    with sqlite3.connect(DB_NAME) as conn:
        cursor = conn.cursor()
        cursor.execute("""
            CREATE TABLE IF NOT EXISTS pedidos (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                cliente_nome TEXT NOT NULL,
                endereco_entrega TEXT NOT NULL,
                valor_total REAL NOT NULL,
                status TEXT DEFAULT 'Pendente'
            )
        """)
        conn.commit()

init_db()
```
##### Modelos Pydantic para validação de dados

```
class PedidoBase(BaseModel):
    cliente_nome: str
    endereco_entrega: str
    valor_total: float

class PedidoCreate(PedidoBase):
    pass

class PedidoUpdateStatus(BaseModel):
    status: str

class PedidoResponse(PedidoBase):
    id: int
    status: str
```

##### Rotas das Api

```
@app.post("/pedidos", response_model=PedidoResponse, status_code=201)
def create_pedido(pedido: PedidoCreate):
    with sqlite3.connect(DB_NAME) as conn:
        cursor = conn.cursor()
        cursor.execute(
            "INSERT INTO pedidos (cliente_nome, endereco_entrega, valor_total) VALUES (?, ?, ?)",
            (pedido.cliente_nome, pedido.endereco_entrega, pedido.valor_total)
        )
        conn.commit()
        pedido_id = cursor.lastrowid
    return {**pedido.dict(), "id": pedido_id, "status": "Pendente"}
```

```
@app.get("/pedidos", response_model=List[PedidoResponse])
def get_pedidos():
    with sqlite3.connect(DB_NAME) as conn:
        conn.row_factory = sqlite3.Row
        cursor = conn.cursor()
        cursor.execute("SELECT * FROM pedidos")
        rows = cursor.fetchall()
    return [dict(row) for row in rows]
```

```
@app.put("/pedidos/{pedido_id}", response_model=PedidoResponse)
def update_status(pedido_id: int, payload: PedidoUpdateStatus):
    with sqlite3.connect(DB_NAME) as conn:
        cursor = conn.cursor()
        cursor.execute("UPDATE pedidos SET status = ? WHERE id = ?", (payload.status, pedido_id))
        conn.commit()
        if cursor.rowcount == 0:
            raise HTTPException(status_code=404, detail="Pedido não encontrado")
        
        cursor.execute("SELECT * FROM pedidos WHERE id = ?", (pedido_id,))
        row = cursor.fetchone()
    return {"id": row[0], "cliente_nome": row[1], "endereco_entrega": row[2], "valor_total": row[3], "status": row[4]}
```

```
@app.delete("/pedidos/{pedido_id}", status_code=204)
def delete_pedido(pedido_id: int):
    with sqlite3.connect(DB_NAME) as conn:
        cursor = conn.cursor()
        cursor.execute("DELETE FROM pedidos WHERE id = ?", (pedido_id,))
        conn.commit()
        if cursor.rowcount == 0:
            raise HTTPException(status_code=404, detail="Pedido não encontrado")
    return None
```

```
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="127.0.0.1", port=8000)
```
