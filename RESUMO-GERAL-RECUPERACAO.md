# Resumo Geral — Infraestrutura de Comunicação (Recuperação)

**CESAR School — Prof. Petrônio Júnior (pglj2@cesar.school)**
Base: Kurose & Ross, *Redes de Computadores e a Internet* (8ª ed.). Resumo gerado a partir da leitura completa dos 7 PDFs da pasta (≈450 slides).

---

## Aula 0 — Apresentação da disciplina (19 slides)

- **Avaliação:** 1ª unidade = prova 100%. 2ª unidade = prova 50% + trabalho 30% + seminário 20%. Pontos extras em algumas atividades; presença nos seminários compõe nota.
- **Segunda chamada:** cobre o assunto **inteiro** e substitui a nota de **uma unidade inteira**.
- Presença cobrada (chamada ao final da aula, faltas não removíveis); prazos diferenciados para alunos com laudo.
- Plano de ensino completo (ementa/cronograma) está no **Google Classroom**.
- **Bibliografia:** Kurose & Ross 8ª ed. (2021) e 6ª ed. (2014); Tanenbaum 6ª ed. (2021).

---

## Aula 1 — Redes de computadores e a Internet (71 slides)

### O que é a Internet
- Componentes: **sistemas finais (hosts)**, **enlaces** (caracterizados pela taxa de transmissão, bits/s), **ISPs**, **protocolos** (padrões documentados em **RFCs**).
- Números: ~850 milhões de sistemas finais permanentes (2011); ~29,3 bilhões de dispositivos (CISCO, 2023).
- **Protocolo (definição para decorar):** define o **formato** e a **ordem** das mensagens trocadas entre entidades comunicantes, e as **ações** realizadas na transmissão/recebimento.

### Borda da rede (acesso)
- **DSL:** linha telefônica dedicada até o DSLAM na central; FDM no fio — voz 0–4 kHz, upstream 4–50 kHz, downstream 50 kHz–1 MHz (assimétrico).
- **Cabo/HFC:** coaxial compartilhado por centenas de casas até nó de fibra → CMTS. Acesso **compartilhado**.
- **FTTH:** ONT (casa) → splitter óptico → OLT (central).
- Empresas: Ethernet/Wi-Fi → switch → roteador de borda. Celular: 3G/4G/5G.
- Meios: guiados (par trançado, coaxial, fibra) × não guiados (rádio, satélite).

### Núcleo da rede
- **Comutação de pacotes:** store-and-forward (recebe o pacote inteiro antes de reenviar; cada salto adiciona L/R), filas e perdas quando demanda > capacidade, tabelas de repasse. Ex.: entradas de 100 Mb/s → saída 1,5 Mb/s = fila.
- **Comutação de circuitos:** reserva fim a fim; **FDM** (faixa de frequência fixa) × **TDM** (slots de tempo); ineficiente nos períodos de silêncio.
- Pacotes = melhor compartilhamento/eficiência; circuitos = bom para tempo real.
- **Rede de redes:** ligar N ISPs diretamente = O(N²) → hierarquia com ISPs regionais, **Tier-1**, **IXPs**, peering e redes de provedores de conteúdo (Google).

### Atraso, perda e vazão (FÓRMULAS)
- **d_nodal = d_proc + d_fila + d_trans + d_prop**
  - d_proc: examinar cabeçalho/erros (µs)
  - d_fila: depende do congestionamento
  - **d_trans = L/R** (L = bits do pacote; R = taxa do enlace)
  - **d_prop = d/s** (d = distância; s ≈ 2–3×10⁸ m/s)
  - NÃO confundir: transmissão depende de L e R; propagação depende de d e s.
- Perda: buffer finito → fila cheia → descarte.
- **Vazão = min(R₁, R₂, …, R_N)** — o enlace **gargalo** limita. Com enlace R compartilhado por 10 fluxos: min(Rs, Rc, R/10).
- **Traceroute/tracert:** mede RTT até cada roteador do caminho (3 sondas por salto; `*` = sem resposta).

### Camadas e encapsulamento
- **OSI (7):** Aplicação, Apresentação, Sessão, Transporte, Rede, Enlace, Física. **TCP/IP (4):** Aplicação, Transporte, Internet, Host/Rede. **Internet (5, Kurose):** Aplicação, Transporte, Rede, Enlace, Física.
- **PDUs:** mensagem (aplicação) → **segmento** (transporte) → **datagrama** (rede) → **quadro/frame** (enlace).
- Switch processa até **enlace**; roteador até **rede**; hosts têm a pilha completa.

### Segurança e história
- 4 ameaças: **malware**, **sniffing** (inspeção de pacotes), **spoofing** (falsificar identidade), **DoS/DDoS**.
- História: anos 1960 comutação de pacotes; **ARPAnet** (1972, 15 nós, demo de Robert Kahn); "internetting"; ~100 mil hosts no fim dos anos 80; **WWW nos anos 90**; era smartphone no séc. XXI.
- Ferramentas: **tracert** e **Wireshark** (filtro `ip.addr == 8.8.8.8`, ICMP Echo request/TTL exceeded).

---

## Aula 2 — Camada de aplicação (116 slides)

### Princípios
- Aplicações rodam **apenas nos sistemas finais** (núcleo não executa código de aplicação).
- **Cliente-servidor:** servidor always-on, IP fixo, datacenters; cliente intermitente, IP dinâmico, não fala com outros clientes. **P2P:** pares consomem E proveem → **autoescalabilidade**; desafios: amigável ao ISP, segurança, incentivos.
- Processos se comunicam por **mensagens** via **sockets** (API entre aplicação e transporte; desenvolvedor controla acima, SO controla abaixo). Endereçamento = **IP + porta**.
- Protocolo de aplicação define: tipos, sintaxe, semântica e regras das mensagens. Abertos (RFC: HTTP, SMTP) × proprietários (Zoom).
- Serviços de transporte: confiabilidade, vazão, temporização, segurança. **TCP:** confiável, fluxo, congestionamento, conexão — sem temporização/banda. **UDP:** nada garante (mas é leve). **TLS:** implementado na **aplicação**, sobre TCP (criptografia, integridade, autenticação).

### HTTP
- TCP **porta 80**, **sem estado** (stateless).
- **Não persistente:** 1 objeto por conexão. **Persistente:** vários objetos.
- **FÓRMULA: Tempo de resposta (não persistente) = 2·RTT + tempo de transmissão** (1 RTT handshake TCP + 1 RTT req/resposta + F/R).
- Requisição: linha (método URL versão) + cabeçalhos + linha em branco + corpo. Métodos: GET, POST, HEAD, PUT, DELETE.
- Códigos: **200 OK, 301 Moved Permanently, 400 Bad Request, 404 Not Found, 505**.
- **Cookies** (4 componentes): Set-cookie na resposta, Cookie na requisição, arquivo no cliente, BD no servidor.
- **Cache web (proxy):** cliente e servidor ao mesmo tempo; cenário clássico LAN 10 Mbps + acesso 1,5 Mbps → cache é a solução barata (vs. aumentar enlace).
- **GET condicional:** `If-Modified-Since` → `304 Not Modified` se não mudou.
- **HTTP/2 (RFC 7540):** multiplexação de frames em 1 conexão TCP (mitiga HOL blocking), priorização, server push.
- **HTTP/3:** sobre **QUIC** (que é protocolo de TRANSPORTE, sobre UDP — pegadinha!); controle de erro/congestionamento **por stream**; TCP+TLS ≈ 2–3 RTT × QUIC ≈ 1 RTT.

### FTP, E-mail, DNS
- **FTP:** 2 conexões TCP — **controle porta 21**, **dados porta 20** (nova conexão de dados por arquivo). Comandos USER/PASS/LIST/RETR/STOR.
- **E-mail:** agentes de usuário + servidores (mailbox + fila) + **SMTP (TCP 25)**. Servidor remetente age como **cliente** SMTP. 3 fases: handshake, transferência, encerramento. Mensagem termina com "." sozinho. ASCII 7 bits → **MIME** (Content-Type, base64).
- SMTP = **push**; HTTP = **pull**.
- Formato RFC 822: From:/To:/Subject: (≠ MAIL FROM/RCPT TO).
- Acesso: **POP3 (110)** baixa-e-apaga, sem estado; **IMAP** pastas no servidor, mantém estado, busca partes; **Webmail (HTTP)**.
- **DNS (UDP 53):** BD distribuído hierárquico + protocolo. Serviços: nome→IP, aliases, MX, distribuição de carga. Não centralizado por: ponto único de falha, tráfego, distância, manutenção.
- Hierarquia: **13 servidores raiz** → **TLD** (.com, .br...) → **autoritativos**; + **servidor local (resolver)** de cada ISP com cache (TTL).
- **Iterativa:** "me diga a quem perguntar" (o local faz o trabalho). **Recursiva:** "resolva para mim" (carga nos níveis altos).
- **Registros (nome, valor, tipo, TTL):** **A** (host→IP), **NS** (domínio→servidor de nomes), **CNAME** (alias→canônico), **MX** (correio).
- Ataques: DDoS na raiz/TLD, envenenamento/redirecionamento, amplificação.

### P2P e CDN (FÓRMULAS)
- BitTorrent: **tracker** mantém lista de pares; troca de **chunks**.
- **Cliente-servidor: Dcs = max{ N·F/us , F/dmin }** (cresce linear com N).
- **P2P: Dp2p = max{ F/us , F/dmin , N·F/(us + Σui) }** (autoescalável).
  - N = nº de clientes; F = tamanho do arquivo; us = upload do servidor; dmin = menor download; Σui = soma dos uploads dos pares.
- Streaming: >65% do tráfego (2024-25); Netflix 15% (2023). Buffer no cliente; **DASH**: vídeo em chunks codificados em várias taxas, cliente escolhe qualidade dinamicamente.
- **CDN:** Enter Deep (clusters pequenos dentro dos ISPs, Akamai) × Bring Home (clusters grandes em IXPs). Redirecionamento **via DNS** (CNAME da origem → CDN → IP do cluster). Seleção de cluster: geográfica (nem sempre a melhor rede) × medição de atraso (exige sondas constantes).

### Portas para decorar
HTTP 80 · FTP 21 (controle) / 20 (dados) · SMTP 25 · POP3 110 · DNS 53 (UDP) · DHCP 67/68 (UDP) · OpenFlow 6653 (TCP)

---

## Aula 3 — Camada de transporte, parte 1 (47 slides)

### Serviços e mux/demux
- Transporte = comunicação lógica **entre processos**; rede = **entre hosts**. Transporte **não roda em roteadores**. Unidade = **segmento**.
- **Demux UDP:** só **(IP destino, porta destino)** identifica o socket. **Demux TCP:** **quádrupla (IP origem, porta origem, IP destino, porta destino)** — conexões distintas mesmo com mesma porta de origem.

### UDP
- Best effort, sem conexão. Cabeçalho **8 bytes**: porta origem, porta destino, **comprimento** (do segmento inteiro), **checksum**.
- Por que existe: sem handshake (menos RTT), sem estado, cabeçalho pequeno, sem controle de congestionamento. Usos: DNS, streaming, SNMP. Confiabilidade pode ser feita na aplicação (ideia do QUIC).
- **Checksum:** soma em complemento de 1 das palavras de 16 bits; receptor soma tudo e espera `1111...1`; detecta erros mas **não garante** (erros podem se cancelar).

### Transferência confiável (rdt) — evolução
- **rdt 1.0** (canal perfeito): só envia e entrega, sem feedback.
- **rdt 2.0** (erros de bits): **ARQ** = detecção de erro (checksum) + **ACK/NAK** + retransmissão. Stop-and-wait. Falha: ACK/NAK corrompido → duplicatas.
- **rdt 2.1:** adiciona **número de sequência (0/1)**; receptor descarta duplicatas (mas re-ACKa). (rdt 2.2 = sem NAK, usa ACK duplicado com seq.)
- **rdt 3.0** (erros + perdas): adiciona **temporizador**; timeout → retransmite; duplicatas resolvidas pelo seq. = **protocolo de alternância de bits**. Funciona com ACK perdido e timeout prematuro.

### Desempenho e pipelining (FÓRMULAS)
- Stop-and-wait: **U = (L/R) / (RTT + L/R)** — utilização baixíssima.
- Pipelining (N pacotes no ar): **U = N·(L/R) / (RTT + L/R)**.

### Go-Back-N × Repetição Seletiva
| Aspecto | GBN | SR |
|---|---|---|
| ACK | **Cumulativo** | **Individual** |
| Fora de ordem | Descarta + reenvia último ACK | **Armazena em buffer** |
| Temporizador | 1 (mais antigo sem ACK) | 1 **por pacote** |
| Timeout | Retransmite **todos** os não confirmados | Retransmite **só** aquele pacote |

---

## Aula 4 — Camada de transporte, parte 2: TCP (53 slides)

### TCP básico
- Orientado à conexão (3-way handshake), **não** é comutação de circuitos (conexão só existe nos hosts). Full-duplex, ponto a ponto, fluxo de bytes ordenado/confiável, pipelined, MSS.
- Cabeçalho: portas, **seq**, **ACK**, flags (SYN, FIN, RST, ACK, URG, PSH), **janela de recepção**, checksum.
- **Seq = nº do 1º byte do segmento; ACK = próximo byte esperado (cumulativo).** ISN aleatório.
- Exemplo Telnet: A envia Seq=42, ACK=79 'C' → B: Seq=79, ACK=43 'C' (eco+carona) → A: Seq=43, ACK=80.

### RTT e timeout (FÓRMULAS — decorar)
- **EstimatedRTT = (1−α)·EstimatedRTT + α·SampleRTT** (α = 0,125) — média móvel exponencial.
- **DevRTT = (1−β)·DevRTT + β·|SampleRTT − EstimatedRTT|** (β = 0,25).
- **TimeoutInterval = EstimatedRTT + 4·DevRTT** (inicial: 1 s, RFC 6298).

### Transferência confiável no TCP
- Timer **único** (segmento mais antigo sem ACK); ACKs cumulativos.
- Cenários: ACK perdido → retransmite após timeout; timeout prematuro → ACK cumulativo resolve; ACK=120 cumulativo dispensa retransmitir o que o ACK=100 perdido cobria.
- Receptor: ACK retardado até 500 ms (em ordem); **ACK duplicado imediato** ao detectar lacuna.
- **Fast retransmit: 3 ACKs duplicados → retransmite sem esperar timeout.**

### Controle de fluxo × congestionamento
- **Fluxo (rwnd):** protege o **receptor** (buffer). Janela zero → sondas de 1 byte.
- **Congestionamento (cwnd):** protege a **rede**. Em trânsito ≤ **min(cwnd, rwnd)**.

### Conexão
- Abertura: **SYN → SYN+ACK → ACK** (o 3º já pode levar dados).
- Fechamento: FIN/ACK dos dois lados; cliente espera em **TIME_WAIT** (~30 s).

### Controle de congestionamento
- Congestionamento = fontes demais para a capacidade → perdas e atrasos. Fim a fim (TCP clássico) × assistido pela rede (**ECN**: roteadores marcam bits, detecta antes da perda, sem pacotes extras).
- **Taxa ≈ cwnd/RTT.**
- **Slow start:** cwnd = 1 MSS; +1 MSS por ACK → **dobra por RTT** (exponencial).
- **Congestion avoidance:** +1 MSS por RTT (linear, AIMD).
- Eventos: **timeout → ssthresh = cwnd/2, cwnd = 1 MSS** (recomeça slow start); **3 dup ACKs → ssthresh = cwnd/2 (+3), fast recovery** (janela cai à metade, cresce linear).
- **Tahoe:** qualquer perda → cwnd = 1. **Reno:** 3 dup ACKs → metade + fast recovery; timeout → 1. (Exemplo clássico: ssthresh 8, perda em cwnd 12 → novo ssthresh 6.)
- **CUBIC:** padrão do Linux; cresce rápido até W_max (última perda) e desacelera perto dele (função cúbica).
- **Vegas:** baseado em **atraso** ("manter o cano cheio, mas não mais que cheio").
- **Equidade:** AIMD converge para R/K por conexão; UDP e conexões paralelas quebram a justiça.

### QUIC
- **RFC 9000**, sobre UDP; base do **HTTP/3 (RFC 9114)**; criptografia do TLS embutida (1 RTT); múltiplos **streams** (sem HOL blocking); confiável; congestionamento TCP-friendly. Adoção: 7% (2017) → 75% do tráfego da Meta (2020) → ~40% da Internet (2025).

---

## Aula 5 — Camada de rede: plano de dados (80 slides)

### Visão geral
- **Repasse (forwarding):** local, plano de **dados**, hardware, ns. **Roteamento (routing):** global, plano de **controle**, software, ms/s.
- **Longest prefix match:** entre as linhas que casam, vence o prefixo com **mais bits fixos**.
- Tradicional: cada roteador calcula sua tabela. **SDN:** controlador remoto calcula e distribui.
- Internet = **best effort** (sem garantias de banda, perda, ordem, atraso). Comparar: ATM CBR/ABR, IntServ, DiffServ.
- **Circuitos virtuais** (ATM, frame relay): caminho + nº de CV **por enlace** (muda a cada salto) + estado nos roteadores; 3 fases (estabelecer, transferir, encerrar). **Datagramas** (Internet): sem estado, repasse só pelo destino.

### Roteador por dentro
- 4 componentes: portas de entrada, **elemento de comutação**, portas de saída, processador de roteamento.
- Entrada: terminação de linha → enlace (desencapsula) → lookup/fila (cópia local da tabela).
- Fabrics: **memória** (1ª geração) < **barramento** < **crossbar** (paralelo).
- Filas: na **saída** (contenção → perda), na **entrada** (fabric lento → **HOL blocking**; degrada a partir de ~58% de carga).
- **Buffer: RTT×C** (ex.: 0,25 s × 10 Gbps = 2,5 Gb); moderno: **RTT×C/√N**.
- Escalonamento: FIFO, **prioridade** (classes), **WFQ** (round-robin ponderado). Descarte: tail drop, **RED** (AQM).
- Neutralidade da rede: sem bloqueio, sem gargalo artificial, sem priorização paga.

### IP
- Camada de rede da Internet: protocolos de roteamento (RIP/OSPF/BGP) + **IP** + **ICMP**.
- **Cabeçalho IPv4:** versão, tam. cabeçalho, tipo serviço, comprimento; **identificador, flags, offset** (fragmentação); **TTL**, protocolo, checksum; IP origem (32 bits), IP destino; opções; dados.
- **Fragmentação:** MTU varia por enlace (**Ethernet = 1500 bytes**); remontagem **só no destino**; identificação + flags + offset.
- **Endereçamento:** IP é da **interface**; **CIDR a.b.c.d/x**; contar sub-redes inclui enlaces entre roteadores. ICANN administra (RFC 2050).
- **DHCP (UDP 67 servidor / 68 cliente):** **discover → offer → request → ACK** (broadcast; yiaddr; lease). Fornece também máscara, gateway, DNS.
- **NAT:** faixas privadas **RFC 1918: 10/8, 172.16/12, 192.168/16**; tabela WAN↔LAN com (IP, porta); troca origem na saída e destino na volta.
- **IPv6:** 128 bits, cabeçalho **fixo de 40 bytes**, **sem fragmentação no caminho**, **sem checksum**, anycast, rótulo de fluxo; ICMPv6 incorpora IGMP. Notação hex com compressão `::`. Transição: **pilha dupla** e **tunelamento** (IPv6 dentro de IPv4).

### SDN / repasse generalizado
- Tabela **match+action** generaliza o repasse; **OpenFlow**: match (12 campos, das camadas 1–4: porta, MACs, VLAN, IPs, protocolo, portas TCP/UDP) + ações (**encaminhar, descartar, modificar, enviar ao controlador**) + contadores.
- Um dispositivo match+action faz papel de roteador, switch, firewall e NAT.
- **Middleboxes:** funções além do roteador IP padrão (NAT, firewall/IDS, caches); implementáveis via **NFV** e nuvem.

---

## Aula 6 — Camada de rede: plano de controle (64 slides)

### Estrutura de controle
- Tabela de repasse (tradicional) / tabela de fluxo (SDN) = elo entre planos.
- **Por roteador:** cada um roda o algoritmo. **Logicamente centralizado (SDN):** controlador calcula; **CA** no dispositivo tem função mínima e não fala com outros CAs. Logicamente ≠ **fisicamente** centralizado (replicado por tolerância a falhas).
- Casos reais: **Google B4** e **Microsoft SWAN** (2013); AT&T 75% virtualizado (2020).

### Algoritmos de roteamento
- Grafo: nós = roteadores, arestas = custos (saltos, banda, atraso, R$...).
- Classificação: global (LS) × descentralizado (DV); estático × dinâmico; sensível × insensível à carga.
- **Link-State (Dijkstra):** difusão de estado de enlace → todos com visão completa → cada nó calcula menores caminhos. **D(v) = min(D(v), D(w) + c(w,v))**. Complexidade **O(n²)**. Resultado: próximo salto por destino.
- **Distance Vector (Bellman-Ford):** iterativo, assíncrono, distribuído; só fala com vizinhos; **D_x(y) = min_v{ c(x,v) + D_v(y) }**; recalcula quando muda custo ou chega vetor novo. (Más notícias lentas → contagem ao infinito.)
- **RIP** = DV, máx. **15 saltos**, converge devagar. **OSPF** = LS, custos definidos pelo administrador, difusão periódica.

### BGP (inter-AS)
- **AS** = roteadores sob mesma administração; **ASN** dado pela ICANN. Hierarquia por **escala + autonomia**.
- BGP roteia por **prefixos**; sessões sobre **TCP**: **eBGP** (entre ASs) e **iBGP** (dentro do AS); gateways rodam ambos.
- Atributos: **AS-PATH** (lista de ASs; evita laços) e **NEXT-HOP** (IP de entrada da rota).
- **Hot potato:** sair do próprio AS o mais rápido possível (menor custo interno) — algoritmo "egoísta".
- **Seleção de rota (ordem):** 1) preferência local (política) → 2) AS-PATH mais curto → 3) NEXT-HOP mais próximo (batata quente) → 4) identificadores.
- **IP-Anycast:** mesmo prefixo anunciado de vários pontos; DNS usa muito; **CDNs evitam** (mudança de rota quebra TCP).
- Intra × inter-AS: política, escalabilidade, desempenho.

### SDN em detalhe
- 4 características: repasse por **fluxo**; **separação** dados/controle; controle **externo** ao switch; rede **programável**.
- Arquitetura: apps de controle → **northbound API** → controlador (network OS) → **southbound API (OpenFlow, TCP 6653)** → switches.
- OpenFlow: controlador→switch (configuração, modificar estado, ler estado, packet-out); switch→controlador (packet-in, estado de porta, fluxo removido).
- História: Ethane (2007) → OpenFlow; NFV substitui middleboxes.

### ICMP
- Mensagens de controle/erro entre hosts e roteadores; **carregadas dentro de datagramas IP**; formato tipo + código + checksum + (cabeçalho IP + 8 primeiros bytes do datagrama que causou o erro).
- Principais: **8/0 = ping request; 0/0 = ping reply; 11/0 = TTL expirado; 3/3 = porta inalcançável**.
- **Traceroute:** UDP com TTL crescente para porta improvável; cada roteador devolve **11/0**; o destino devolve **3/3** → sinal de parada.

### SNMP / Gerenciamento
- 5 componentes: servidor gerenciador, dispositivo gerenciado, dados (MIB), agente, protocolo.
- SNMP: **request/response** (polling) e **trap** (notificação assíncrona do agente). Alternativas: CLI, **NETCONF/YANG**.

---

## Temas transversais que mais caem

1. **Fórmulas:** atrasos (L/R, d/s), vazão min(), utilização stop-and-wait/pipelining, tempo HTTP (2RTT+F/R), Dcs/Dp2p, EstimatedRTT/DevRTT/Timeout, taxa ≈ cwnd/RTT, buffer RTT×C/√N.
2. **Tabelas de comparação:** TCP × UDP; GBN × SR; Tahoe × Reno; LS × DV; RIP × OSPF × BGP; iterativa × recursiva (DNS); circuito × pacote; persistente × não persistente.
3. **Portas:** 80, 21/20, 25, 110, 53, 67/68, 6653.
4. **Sequências de protocolo:** 3-way handshake, DHCP DORA, diálogo SMTP, consulta DNS, anúncio BGP, traceroute com ICMP.
5. **Pegadinhas marcadas nos slides:** QUIC é transporte; TLS é implementado na aplicação; conexão TCP não é circuito; logicamente ≠ fisicamente centralizado; IP é da interface; checksum não garante integridade; slow start é exponencial.
