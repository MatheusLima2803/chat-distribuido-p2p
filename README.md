Markdown
# 💬 Chat Distribuído Peer-to-Peer (P2P)

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![RPyC](https://img.shields.io/badge/RPC-RPyC-FF6F61?style=for-the-badge&logo=google-cloud&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)

Um sistema robusto de chat distribuído *peer-to-peer* com comunicação baseada em RPC, difusão de mensagens e consistência de histórico replicado. Desenvolvido como projeto final para a disciplina de **Algoritmos e Estruturas de Dados II** no Instituto de Informática (INF) da **Universidade Federal de Goiás (UFG)**.

O núcleo do projeto consiste em resolver problemas clássicos de sistemas distribuídos, como concorrência, ordenação de eventos na ausência de um relógio global físico, consenso e tolerância a falhas.

---

## 📱 Demonstração

### Tela Principal
![Tela Principal](images/tela-principal.jpeg)

### Menor Caminho Encontrado
![Rota Calculada](images/rota-calculated.jpeg)

### Visualização do Grafo
![Zoom no Mapa](images/zoom-mapa.jpeg)

---

## 🚀 Arquiteturas Implementadas

O sistema disponibiliza duas abordagens de arquitetura e coordenação distribuída para fins comparativos:

### ⚡ Parte 3: Ordenação Total (Relógios de Lamport)
* **Conceito:** Garante consistência sequencial estrita. Todas as mensagens aparecem exatamente na mesma ordem cronológica para todos os usuários da rede, sem depender de relógios físicos.
* **Mecanismo:** Cada evento recebe um timestamp lógico. O buffer de *holdback* retém as mensagens e uma thread de entrega verifica sua estabilidade a cada 100ms antes de exibi-las na ordem determinística: `(lamport_ts, origin_node)`.

### 👑 Parte 4: Primary-Backup (Algoritmo de Eleição Bully)
* **Conceito:** Arquitetura com coordenação centralizada e tolerância a falhas. Um líder sequencia todas as mensagens recebidas e as replica para os nós de backup.
* **Mecanismo:** Garante menor latência e entrega imediata (sem buffer de holdback). Se o monitor de *heartbeat* detectar a queda do líder, os nós iniciam de forma autônoma o **Algoritmo de Bully** para eleger um novo coordenador em até 5 segundos.

---

## 🛠️ Recursos Funcionais

- **Rede P2P Descentralizada:** Comunicação simétrica via RPC utilizando a biblioteca `rpyc`.
- **Descoberta Dinâmica (Discovery):** Inicialização leve com nó de *Bootstrap* e propagação automatizada via mensagens `JOIN`.
- **Tolerância a Falhas:** Monitoramento ativo por *Heartbeats* (timeout de 60s com tratamento de jitter).
- **Consistência de Dados:** Histórico totalmente replicado e persistido localmente em disco no formato JSON.
- **Confiabilidade:** Confirmações de leitura em tempo real e cache de IDs de mensagens para prevenção estrita de duplicatas (*de-duplication*).

---

## 📊 Desempenho e Complexidade Algorítmica

A tabela abaixo apresenta as métricas observadas durante as simulações práticas de estresse da rede:

| Cenário | Tempo de Resposta | Eficiência de Mensagens | Abordagem Teórica |
| :--- | :--- | :--- | :--- |
| **Parte 3 (Lamport)** | ~100ms (Ajustado pela thread) | ~ $N \cdot \log(N)$ chamadas RPC | Comunicação Descentralizada |
| **Parte 4 (Bully)** | < 17 ms (Imediato) | $N$ chamadas RPC diretas | Coordenação Centralizada |

*O projeto superou com ampla margem os requisitos acadêmicos mínimos de estabilidade e tempo de resposta sob concorrência.*

---

## 🏗️ Arquitetura do Sistema

O projeto adota uma estrutura modular clara, isolando os protocolos de rede das camadas de visualização:

```text
src/
├── constCS.py          # Parâmetros globais (timeouts, portas, fanout de difusão)
├── node.py             # Implementação do Nó P2P (Algoritmo de Lamport)
├── node_election.py    # Implementação do Nó P2P (Algoritmo de Bully)
├── client_ui.py        # Interface gráfica do usuário (compatível com ambas as versões)
└── start_bootstrap.bat # Utilitário de inicialização automática do Bootstrap