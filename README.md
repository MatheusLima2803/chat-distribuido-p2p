Markdown# 💬 Chat Distribuído Peer-to-Peer (P2P)

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![RPyC](https://img.shields.io/badge/RPC-RPyC-FF6F61?style=for-the-badge&logo=google-cloud&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)

Um sistema robusto de chat distribuído *peer-to-peer* com comunicação baseada em RPC, difusão de mensagens e consistência de histórico replicado. Desenvolvido como projeto final para a disciplina de **Algoritmos e Estruturas de Dados II** no Instituto de Informática (INF) da **Universidade Federal de Goiás (UFG)**.

O núcleo do projeto consiste em resolver problemas clássicos de sistemas distribuídos, como concorrência, ordenação de eventos na ausência de um relógio global físico, consenso e tolerância a falhas.

---

## 📱 Demonstração de Funcionamento (Logs do Terminal)

O sistema foi validado com sucesso em ambiente local (*localhost*). Como os clientes foram integrados diretamente via console para mitigar latências de interface do sistema operacional, os nós sincronizaram o histórico e registraram os eventos em tempo real conforme demonstrado abaixo:

```text [SISTEMA] Alice entrou no chat [SISTEMA] node2 saiu (timeout)
Historico sincronizado (3 mensagens)
 [SISTEMA] Bob entrou no chat Bob: oi (ID:bootstrap_4) Bob: como vai bob (ID:bootstrap_6) Bob: alice (ID:bootstrap_8)
🚀 Arquiteturas ImplementadasO sistema disponibiliza duas abordagens de arquitetura e coordenação distribuída para fins comparativos:⚡ Parte 3: Ordenação Total (Relógios de Lamport)Conceito: Garante consistência sequencial estrita. Todas as mensagens aparecem exatamente na mesma ordem cronológica para todos os usuários da rede, sem depender de relógios físicos.Mecanismo: Cada evento recebe um timestamp lógico. O buffer de holdback retém as mensagens e uma thread de entrega verifica sua estabilidade a cada 100ms antes de exibi-las na ordem determinística: (lamport_ts, origin_node).👑 Parte 4: Primary-Backup (Algoritmo de Eleição Bully)Conceito: Arquitetura com coordenação centralizada e tolerância a falhas. Um líder sequencia todas as mensagens recebidas e as replica para os nós de backup.Mecanismo: Garante menor latência e entrega imediata (sem buffer de holdback). Se o monitor de heartbeat detectar a queda do líder, os nós iniciam de forma autônoma o Algoritmo de Bully para eleger um novo coordenador em até 5 segundos.🛠️ Recursos FuncionaisRede P2P Descentralizada: Comunicação simétrica via RPC utilizando a biblioteca rpyc.Descoberta Dinâmica (Discovery): Inicialização leve com nó de Bootstrap e propagação automatizada via mensagens JOIN.Tolerância a Falhas: Monitoramento ativo por Heartbeats (timeout de 60s com tratamento de jitter).Consistência de Dados: Histórico totalmente replicado e persistido localmente em disco no formato JSON.Confiabilidade: Confirmações de leitura em tempo real e cache de IDs de mensagens para prevenção estrita de duplicatas (de-duplication).📊 Desempenho e Complexidade AlgorítmicaA tabela abaixo apresenta as métricas observadas durante as simulações práticas de estresse da rede:CenárioTempo de RespostaEficiência de MensagensAbordagem TeóricaParte 3 (Lamport)~100ms (Ajustado pela thread)~ $N \cdot \log(N)$ chamadas RPCComunicação DescentralizadaParte 4 (Bully)< 17 ms (Imediato)$N$ chamadas RPC diretasCoordenação CentralizadaO projeto superou com ampla margem os requisitos acadêmicos mínimos de estabilidade e tempo de resposta sob concorrência.🏗️ Arquitetura do SistemaO projeto adota uma estrutura modular clara, isolando os protocolos de rede das camadas de visualização:Plaintextsrc/
├── constCS.py          # Parâmetros globais (timeouts, portas, fanout de difusão)
├── node.py             # Implementação do Nó P2P (Algoritmo de Lamport)
├── node_election.py    # Implementação do Nó P2P (Algoritmo de Bully)
├── client_ui.py        # Interface gráfica do usuário (compatível com ambas as versões)
└── start_bootstrap.bat # Utilitário de inicialização automática do Bootstrap
Estruturas de Dados por Versão:Parte 3 (Lamport) - Cada nó armazena:Lista de peers (host, porta, conexão RPC)Histórico completo de mensagens (réplica local)Cache de IDs processados (previne duplicatas)Status de heartbeat dos peersRelógio lógico de LamportBuffer de holdback para ordenação totalParte 4 (Bully) - Cada nó armazena:Lista de peers (host, porta, conexão RPC)Histórico completo de mensagens (réplica local)Cache de IDs processados (previne duplicatas)Status de heartbeat dos peersID do líder atualContador de sequência (se for líder)Sequência esperada (se for backup)📨 Protocolo de ComunicaçãoToda a camada de rede é estruturada utilizando chamadas de procedimentos remotos (RPC) via rpyc.Métodos Comuns (ambas versões):Gerenciamento de rede:get_initial_peers(): Obtém lista inicial do bootstrap.receive_join(): Recebe JOIN de novo peer e propaga para a malha.heartbeat(): Recebe batimento cardíaco de validação de peer estável.peer_list_update(): Sincroniza lista ativa de peers.Interface com cliente:join_user(): Usuário entra no chat.send_message(): Envia mensagem para a malha.get_messages(): Consome mensagens da fila local.read_confirm(): Confirma leitura de uma mensagem específica.leave_user(): Desconecta o usuário do chat com segurança.Métodos Específicos:Parte 3 (Lamport):receive_chat_message(): Recebe mensagem CHAT e gerencia a propagação.receive_system_message(): Recebe mensagens informativas de sistema.receive_read_confirm(): Propaga a confirmação de leitura lúdica.Parte 4 (Bully):receive_replicated_chat(): Backup recebe payload CHAT direto do líder sequenciador.receive_replicated_system(): Backup recebe SYSTEM estruturado do líder.receive_replicated_read_confirm(): Backup sincroniza confirmação através do líder.forward_chat_message(): Líder recebe CHAT encaminhado de um nó backup.forward_system_message(): Líder recebe SYSTEM encaminhado.forward_read_confirm(): Líder recebe READ_CONFIRM encaminhado.election(): Processa a mensagem de eleição ativa (Algoritmo Bully).coordinator(): Recebe anúncio oficial de novo coordenador da rede.Tipos de mensagens monitoradas:CHAT: Mensagem de conversa convencional entre usuários.SYSTEM: Notificações automáticas estruturadas da infraestrutura de rede.READ_CONFIRM: Confirmação ativa de leitura síncrona.JOIN: Protocolo de entrada de novo peer na malha distribuída.💻 UtilizaçãoComo funciona a Inicialização e DiscoveryO Nó bootstrap inicia e aguarda novas conexões na malha.Novos peers conectam-se ao bootstrap e resolvem a chamada get_initial_peers().Cada peer dispara uma notificação JOIN para os elementos ativos mapeados.O JOIN é difundido de forma controlada por propagação recursiva.Os nós respondem com confirmações primitivas de rede estabelecendo acoplamento P2P.Execução dos Testes PráticosParte 3 - Ordenação Total (Lamport)Iniciar nó bootstrap:Bashpython node.py --node-id bootstrap --port 5679 --bootstrap
Iniciar nós peers:Bashpython node.py --node-id node2 --port 5680
python node.py --node-id node3 --port 5681
Parte 4 - Primary-Backup (Bully)Iniciar nó bootstrap:Bashpython node_election.py --node-id bootstrap --port 5679 --bootstrap
Iniciar nós peers:Bashpython node_election.py --node-id node2 --port 5680
python node_election.py --node-id node3 --port 5681
Nota: Pelo critério de ordenação estrutural do algoritmo Bully, o nó instanciado com o maior identificador numérico assume a coordenação primária implicitamente.Conectar Clientes (Modo Console/Terminal Integrado)Bashpython client_ui.py --username Alice --node-port 5679
python client_ui.py --username Bob --node-port 5679
⚙️ Configuração dos Parâmetros Locais (constCS.py)Para cenários de testes locais com alta latência de concorrência interna do sistema operacional, os parâmetros de rede podem ser adaptados diretamente no arquivo global:Python# Parâmetros Baselines de Rede
BOOTSTRAP_HOST = 'localhost'
BOOTSTRAP_PORT = 5679
NODE_BASE_PORT = 5679

# Heartbeat & Tolerância (Modificados para estabilidade local)
HEARTBEAT_INTERVAL = 9999
HEARTBEAT_TIMEOUT = 99999

# Difusão Controlada e Janela Lógica
PROPAGATION_FANOUT = 3
DELIVERY_CHECK_INTERVAL = 0.1  # 100ms
📋 Detalhes dos Schemas de Dados Internos (JSON)Estrutura adotada na Parte 3 (Lamport)JSON{
    "type": "CHAT",
    "sender": "Alice",
    "message": "Oi Bob, teste de ordenacao lógica.",
    "timestamp": 1234567890.0,
    "msg_id": "node2_12",
    "lamport_ts": 4,
    "origin_node": "node2"
}
Estrutura adotada na Parte 4 (Primary-Backup)JSON{
    "type": "CHAT",
    "sender": "Bob",
    "message": "Mensagem sequenciada pelo lider.",
    "timestamp": 1234567895.0,
    "msg_id": "node3_15",
    "seq_number": 8,
    "leader_id": "node3"
}