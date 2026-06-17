# 💬 Chat Distribuído Peer-to-Peer (P2P)

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![RPyC](https://img.shields.io/badge/RPC-RPyC-FF6F61?style=for-the-badge&logo=google-cloud&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)

Um sistema robusto de chat distribuído *peer-to-peer* com comunicação baseada em RPC, difusão de mensagens e consistência de histórico replicado. Desenvolvido como projeto prático para a disciplina de **Sistemas Distribuídos** no Instituto de Informática (INF) da **Universidade Federal de Goiás (UFG)**.

O núcleo do projeto consiste em resolver problemas clássicos de sistemas distribuídos, como concorrência, ordenação de eventos na ausência de um relógio global físico, consenso e tolerância a falhas.

---

## 📱 Demonstração de Funcionamento (Logs do Terminal)

O sistema foi validado em ambiente local (*localhost*), onde os nós clientes foram inicializados e sincronizados com sucesso através do nó de *Bootstrap*, garantindo a consistência total no envio de mensagens e histórico em tempo real:

```text [SISTEMA] Alice entrou no chat [SISTEMA] node2 saiu (timeout)
Historico sincronizado (3 mensagens)
 [SISTEMA] Bob entrou no chat Bob: oi (ID:bootstrap_4) Bob: como vai bob (ID:bootstrap_6) Bob: alice (ID:bootstrap_8)
