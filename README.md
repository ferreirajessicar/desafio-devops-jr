# PicPay DevOps Challenge — Solução

Este repositório contém minha solução para o desafio proposto pelo PicPay, que consiste em corrigir e executar uma aplicação multicontainer utilizando Docker e Docker Compose.

A aplicação é composta por quatro serviços principais:

- **Frontend (Node.js)**: Interface web acessível via navegador.
- **Writer (Python)**: Armazena dados recebidos no Redis.
- **Reader (Go)**: Lê os dados armazenados no Redis.
- **Redis**: Banco de dados em memória, utilizado como cache/armazenamento.

---

## Etapas de Correção e Configuração

### Docker Compose

- Corrigido nome do serviço `redis` e adicionada a porta padrão (`6379`).
- Declaradas corretamente as redes `backend` e `frontend`.
- Removido o `reader` da rede `frontend`, pois não havia necessidade.
- Atualizada a versão do Compose, removendo campos obsoletos.

### Writer (Python)

- Removido `ENTRYPOINT` incorreto e ajustado para `CMD ["python", "main.py"]`.
- Adicionado o módulo `redis` ao `requirements.txt`.
- Dockerfile atualizado para instalar dependências corretamente.

### Reader (Go)

- Corrigido `WORKDIR` para `/app` no Dockerfile.
- Adicionado comando para criação de `go.mod` caso não exista.
- Ajustada a versão base da imagem do Go para 1.23.0.
- Corrigido uso da função `client.Get` no código-fonte.

### Web (Frontend)

- Corrigido mapeamento e exposição da porta `3000`.

---

## Como Executar

1. Certifique-se de que o Docker e Docker Compose estão instalados.
2. Execute o seguinte comando na raiz do projeto:

```bash
docker compose up --build
```

3. Acesse no navegador: [http://localhost:5000](http://localhost:5000)

---

## Diagrama de Funcionamento

```mermaid
graph TD
    Browser[User (Browser)]
    subgraph Frontend [Frontend Service]
        FE["Home (Node.js App)"]
    end
    subgraph Backend [Backend Services]
        Reader["Reader (Go App)"]
        Writer["Writer (Python App)"]
    end
    subgraph Database [Database]
        Redis[(Redis - Key-Value Store)]
    end
    Browser --> FE
    FE -->|Status Check| Reader
    FE -->|Status Check| Writer
    FE -->|GET /data| Reader
    FE -->|POST /write| Writer
    Writer -->|Set SHAREDKEY| Redis
    Reader -->|Get SHAREDKEY| Redis
```

---

## Resultado Final

- Todos os containers iniciam corretamente.
- A comunicação entre serviços está funcionando conforme esperado.
- A aplicação é acessível e responde às interações via navegador.
