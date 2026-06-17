# 💬 Chat Distribuído Peer-to-Peer (P2P)

Sistema distribuído de chat peer-to-peer desenvolvido em Python com comunicação via RPC, replicação de histórico, tolerância a falhas e ordenação consistente de mensagens.

O projeto explora conceitos fundamentais de Sistemas Distribuídos, incluindo relógios lógicos de Lamport, eleição de líder pelo algoritmo Bully, replicação de dados, heartbeats, descoberta dinâmica de nós e coordenação entre processos distribuídos.

## 📸 Demonstração

### Interface do Sistema

![Tela Principal](images/tela-principal.png)

### Comunicação entre Nós

![Logs do Sistema](images/logs-distribuidos.png)

---

## 🚀 Principais Funcionalidades

* Comunicação Peer-to-Peer (P2P)
* Descoberta dinâmica de nós
* Replicação distribuída de histórico
* Prevenção de mensagens duplicadas
* Persistência local em JSON
* Monitoramento por Heartbeat
* Tolerância a falhas
* Eleição automática de líder
* Ordenação total de mensagens

---

## 🏗️ Arquiteturas Implementadas

### Parte 1 — Ordenação Total (Relógios de Lamport)

Implementação baseada em timestamps lógicos para garantir que todos os nós visualizem as mensagens na mesma ordem, independentemente do momento de entrega.

**Conceitos aplicados:**

* Relógios lógicos de Lamport
* Ordenação total
* Consistência sequencial
* Buffers de holdback

### Parte 2 — Primary-Backup com Algoritmo Bully

Implementação baseada em coordenação centralizada através de um líder responsável pela sequência global das mensagens.

**Conceitos aplicados:**

* Algoritmo de eleição Bully
* Replicação Primary-Backup
* Heartbeats
* Failover automático
* Coordenação distribuída

---

## 🛠️ Tecnologias Utilizadas

* Python
* RPyC (Remote Procedure Calls)
* JSON
* Threads
* Programação Concorrente
* Sistemas Distribuídos

---

## 📊 Resultados Obtidos

| Cenário                | Tempo Médio |
| ---------------------- | ----------- |
| Lamport                | ~100 ms     |
| Bully / Primary-Backup | < 17 ms     |

O sistema atendeu com ampla margem os requisitos acadêmicos de estabilidade, consistência e desempenho sob concorrência.

---

## 📂 Estrutura do Projeto

```text
src/
├── constCS.py
├── node.py
├── node_election.py
├── client_ui.py
└── start_bootstrap.bat
```

---

## ⚙️ Como Executar

Instalar dependências:

```bash
pip install -r requirements.txt
```

Executar nó bootstrap:

```bash
python node.py --node-id bootstrap --port 5679 --bootstrap
```

Executar nós participantes:

```bash
python node.py --node-id node2 --port 5680

python node.py --node-id node3 --port 5681
```

Conectar clientes:

```bash
python client_ui.py --username Alice --node-port 5679

python client_ui.py --username Bob --node-port 5679
```

---

## 🎓 Contexto Acadêmico

Projeto Final da disciplina Sistemas Distribuídos.

Instituto de Informática (INF)

Universidade Federal de Goiás (UFG)
