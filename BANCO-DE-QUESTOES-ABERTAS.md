# Banco de Questões Abertas — Recuperação (com respostas-modelo)

Prova dissertativa, todo o conteúdo (Aulas 1–6). Meta: **nota ≥ 5**.

## Como usar este banco (método para prova aberta)

1. **Uma sessão por aula.** Leia a seção da aula no `RESUMO-GERAL-RECUPERACAO.md` (15–20 min), feche o arquivo, e responda **por escrito, sem consultar**, as questões 🔴 daquela aula.
2. Compare com a resposta-modelo. Marque as que errou e refaça no dia seguinte.
3. Só passe para as 🟡 quando acertar todas as 🔴 da aula. As 🟢 são o polimento final.
4. **Regra de ouro da dissertativa:** toda resposta tem 3 partes — *definição* + *como funciona (passo a passo)* + *exemplo ou número*. Mesmo sem saber tudo, escreva o que sabe: em questão aberta, resposta parcial vale ponto; em branco vale zero.
5. As questões marcadas com 💬 vieram de perguntas que o próprio professor deixou nos slides — probabilidade altíssima.

**Prioridades:** 🔴 quase certo que cai (núcleo de cada aula) · 🟡 provável · 🟢 possível

**Matemática da nota 5:** dominar todas as 🔴 cobre o tema central de cada uma das 6 aulas. Numa prova de 5, 10 ou 15 questões distribuídas pelo conteúdo, isso sozinho já encosta na metade dos pontos; as 🟡 são a margem de segurança.

---

## AULA 1 — Redes e Internet

### 🔴 Q1. Defina "protocolo" no contexto de redes.
**R:** Um protocolo define o **formato** e a **ordem** das mensagens trocadas entre duas ou mais entidades comunicantes, bem como as **ações** realizadas na transmissão e/ou no recebimento de uma mensagem ou outro evento. Ex.: o HTTP define o formato das requisições/respostas, a ordem (requisição antes da resposta) e as ações (servidor devolve o objeto ou um código de erro).

### 🔴 Q2. Compare comutação de circuitos e comutação de pacotes. Em que situação cada uma é preferível?
**R:** Na **comutação de circuitos**, os recursos do caminho fim a fim são **reservados** para a chamada (por FDM — cada chamada ganha uma faixa de frequência; ou TDM — cada chamada ganha slots de tempo periódicos). Garante desempenho constante, mas desperdiça capacidade nos períodos de silêncio. Na **comutação de pacotes**, não há reserva: os dados são enviados em pacotes que compartilham os enlaces sob demanda (multiplexação estatística), com transmissão *store-and-forward* e filas nos roteadores — mais eficiente no uso da banda, porém com atraso variável e possibilidade de perda. **Circuitos** são adequados a serviços de tempo real com garantias; **pacotes** são melhores para tráfego de dados em rajadas (caso da Internet).

### 🔴 Q3. Cite e explique os quatro componentes do atraso nodal. Dê as fórmulas de transmissão e propagação.
**R:** `d_nodal = d_proc + d_fila + d_trans + d_prop`.
- **Processamento (d_proc):** examinar o cabeçalho, verificar erros de bits e decidir o enlace de saída (µs).
- **Fila (d_fila):** espera na fila do enlace de saída; depende do congestionamento, varia pacote a pacote.
- **Transmissão (d_trans = L/R):** tempo de "empurrar" todos os L bits do pacote no enlace de taxa R bps.
- **Propagação (d_prop = d/s):** tempo de um bit percorrer a distância d do enlace, na velocidade s do meio (~2–3×10⁸ m/s).
**Não confundir:** transmissão depende do tamanho do pacote e da velocidade do enlace; propagação depende da distância e do meio físico.

### 🔴 Q4. (Cálculo) Um pacote de 1000 bytes será enviado por um enlace de 2 Mb/s e 500 km, com velocidade de propagação 2,5×10⁸ m/s. Calcule o atraso de transmissão, o de propagação e o total (desprezando processamento e fila).
**R:** L = 1000 × 8 = 8000 bits.
- d_trans = L/R = 8000 / 2×10⁶ = **4 ms**
- d_prop = d/s = 5×10⁵ / 2,5×10⁸ = **2 ms**
- Total = 4 + 2 = **6 ms**.
(Com store-and-forward e mais um roteador no meio com enlaces iguais, somaria mais um L/R: 4 ms extras.)

### 🔴 Q5. Apresente as 5 camadas do modelo Internet, a função de cada uma e o nome da unidade de dados (PDU) em cada camada.
**R:**
1. **Aplicação** — protocolos das aplicações (HTTP, SMTP, DNS); PDU = **mensagem**.
2. **Transporte** — comunicação lógica entre **processos** (TCP/UDP); PDU = **segmento**.
3. **Rede** — comunicação lógica entre **hospedeiros**, roteamento/repasse (IP); PDU = **datagrama**.
4. **Enlace** — transferência entre nós vizinhos no mesmo enlace (Ethernet, Wi-Fi); PDU = **quadro (frame)**.
5. **Física** — bits no meio físico.
No encapsulamento, cada camada acrescenta seu cabeçalho à PDU da camada de cima. Um **switch** processa até a camada de **enlace**; um **roteador** até a camada de **rede**; os **hosts** têm a pilha completa.

### 🟡 Q6. O que é transmissão store-and-forward e qual sua consequência no atraso?
**R:** O comutador precisa **receber o pacote inteiro** antes de começar a retransmiti-lo no próximo enlace. Consequência: cada salto intermediário adiciona um tempo de transmissão L/R completo. Ex.: 2 enlaces de taxa R → o pacote leva 2·L/R (mais propagação) da origem ao destino.

### 🟡 Q7. Defina vazão fim a fim e enlace gargalo. Se um servidor (Rs = 100 Mb/s) atende um cliente (Rc = 50 Mb/s) através de um enlace de núcleo de R = 200 Mb/s compartilhado igualmente por 10 fluxos, qual a vazão de cada fluxo?
**R:** Vazão fim a fim é a taxa efetiva de bits recebidos, limitada pelo **enlace gargalo** (o de menor taxa do caminho): vazão = min(R₁,…,R_N). No exemplo: min(100, 50, 200/10) = min(100, 50, 20) = **20 Mb/s** — o gargalo é a fração do enlace compartilhado.

### 🟡 Q8. Cite as quatro classes de ameaça à segurança apresentadas.
**R:** (1) **Malware** distribuído pela Internet (vírus, worms, botnets); (2) **inspeção de pacotes (sniffing)** — leitura de pacotes alheios em meios de difusão; (3) **falsificação de identidade (spoofing)** — ex.: enviar pacotes com IP de origem falso; (4) **comprometimento de serviços (DoS/DDoS)** — inundar servidor/infraestrutura com tráfego para torná-lo indisponível.

### 🟢 Q9. Compare DSL, cabo (HFC) e FTTH.
**R:** **DSL:** usa a linha telefônica individual (par trançado) até o DSLAM da central; canal **dedicado**; frequências divididas — voz 0–4 kHz, upstream 4–50 kHz, downstream 50 kHz–1 MHz (assimétrico). **Cabo/HFC:** coaxial **compartilhado** por centenas de residências até um nó de fibra, e fibra até o CMTS do operador — banda dividida entre vizinhos. **FTTH:** fibra da central até a residência (ONT na casa, splitter óptico passivo, OLT na central) — maiores taxas.

### 🟢 Q10. Por que não se conectam todos os ISPs de acesso diretamente entre si? Como a Internet se organiza?
**R:** Ligação direta entre N ISPs exigiria O(N²) conexões — não escala. A solução é hierárquica: ISPs de acesso → ISPs **regionais** → ISPs **Tier-1** (topo, não pagam trânsito), interligados em **IXPs** (pontos de troca de tráfego) e por **peering**; além disso, provedores de conteúdo (ex.: Google) têm redes próprias ligadas direto a IXPs/ISPs para encurtar o caminho aos usuários.

### 🟢 Q11. O que o traceroute mede e o que significa uma linha com `*`?
**R:** Mede o atraso de ida e volta (RTT) da origem até **cada roteador** do caminho, com 3 sondas por salto. `*` significa que o roteador daquele salto **não respondeu** à sonda dentro do tempo limite — não indica necessariamente falha (muitos roteadores não respondem por política).

---

## AULA 2 — Camada de Aplicação

### 🔴 Q1. Compare as arquiteturas cliente-servidor e P2P. 💬 Como cada uma obtém escalabilidade?
**R:** **Cliente-servidor:** servidor sempre ativo, com IP fixo, geralmente em datacenters; clientes intermitentes, com IP dinâmico, que só falam com o servidor. Escala **multiplicando servidores** (datacenters). **P2P:** sem servidor sempre ativo; pares comunicam-se diretamente e **consomem E proveem** serviço — cada novo par adiciona demanda mas também capacidade de upload (**autoescalabilidade**). Desafios do P2P: ser amigável ao ISP, segurança e incentivos à contribuição. 💬 Um mesmo processo pode ser cliente e servidor ao mesmo tempo (é o caso do P2P).

### 🔴 Q2. Diferencie conexões HTTP persistentes e não persistentes e calcule o tempo para obter 1 objeto de tamanho F num enlace de taxa R com conexão não persistente.
**R:** **Não persistente:** no máximo 1 objeto por conexão TCP — cada objeto paga abertura e fechamento de conexão. **Persistente:** vários objetos na mesma conexão. Tempo (não persistente) = **2·RTT + F/R**: 1 RTT do handshake TCP + 1 RTT da requisição/início da resposta + tempo de transmissão do arquivo. Com N objetos em conexões não persistentes seriam ~N·(2RTT) + transmissões; a persistente reduz para ~1 RTT por objeto adicional.

### 🔴 Q3. O HTTP é um protocolo "sem estado". O que isso significa e como os cookies contornam isso?
**R:** Sem estado = o servidor **não guarda informação sobre requisições anteriores** do cliente. Cookies criam estado com 4 componentes: (1) cabeçalho **`Set-cookie:`** na resposta HTTP (servidor cria um ID); (2) cabeçalho **`Cookie:`** nas requisições seguintes; (3) **arquivo de cookies** no computador do usuário, gerido pelo navegador; (4) **banco de dados** no site associando o ID ao usuário. Assim o servidor reconhece o usuário entre requisições (sessões, carrinho) — com implicações de privacidade.

### 🔴 Q4. Explique as consultas DNS iterativa e recursiva. Quem "faz o trabalho" em cada uma?
**R:** Na **iterativa**, o servidor consultado responde "**me diga a quem perguntar**": o servidor **local (resolver)** consulta a raiz → recebe o endereço do TLD → consulta o TLD → recebe o autoritativo → consulta o autoritativo → recebe o IP e repassa ao host. O trabalho fica com o **servidor local**. Na **recursiva**, cada servidor **repassa a consulta adiante** ("resolva para mim"): local → raiz → TLD → autoritativo, e a resposta volta pela mesma cadeia — a carga sobe para os **níveis altos da hierarquia** (por isso a raiz prefere iterativa).

### 🔴 Q5. Descreva a hierarquia de servidores DNS e os 4 tipos de registro (RR).
**R:** Hierarquia: **servidores raiz** (13 identidades lógicas) → **servidores TLD** (.com, .org, .br…) → **servidores autoritativos** (da organização, com os registros oficiais); além deles, o **servidor local/resolver** do ISP, que recebe as consultas dos hosts e mantém **cache** (entradas expiram por TTL). Registros (nome, valor, tipo, TTL): **A** = hostname → IP; **NS** = domínio → servidor de nomes autoritativo; **CNAME** = apelido → nome canônico; **MX** = domínio → servidor de correio.

### 🟡 Q6. 💬 Por que o DNS não é centralizado?
**R:** Quatro razões: (1) **ponto único de falha**; (2) **volume de tráfego** insuportável para um só servidor; (3) um BD central ficaria **distante** de muitos clientes (latência); (4) **manutenção** impraticável. Em resumo: **não escala**.

### 🟡 Q7. Uma instituição tem LAN de 10 Mb/s e enlace de acesso de 1,5 Mb/s saturado. Compare as duas soluções apresentadas em aula.
**R:** (1) **Aumentar o enlace de acesso** (ex.: para 10 Mb/s): resolve, mas é **caro**. (2) **Instalar um cache web (proxy) institucional** na LAN: a fração das requisições com acerto (hit rate) é atendida localmente a 10 Mb/s, reduzindo o tráfego no enlace de acesso e o tempo médio de resposta — solução **barata**. O cache atua como servidor (para o cliente) e cliente (para o servidor de origem).

### 🟡 Q8. O que é o GET condicional e para que serve o código 304?
**R:** Mecanismo para o cache verificar se sua cópia está atualizada: envia a requisição com **`If-Modified-Since: <data>`**. Se o objeto **não mudou**, o servidor responde **`304 Not Modified`** sem enviar o objeto (o cache usa a cópia local); se mudou, responde `200 OK` com a versão nova. Economiza banda e tempo.

### 🟡 Q9. Descreva o percurso de um e-mail de Alice para Bob e diferencie SMTP de HTTP.
**R:** (1) Alice compõe no **agente de usuário**; (2) a mensagem vai ao **servidor de correio de Alice** (fila de saída); (3) o lado **cliente SMTP** do servidor de Alice abre conexão **TCP porta 25** com o servidor de Bob; (4) transfere a mensagem (fases: handshake → transferência → encerramento; fim da mensagem = linha só com "."); (5) o servidor de Bob deposita na **caixa postal**; (6) Bob lê com POP3/IMAP/HTTP. **SMTP é push** (empurra para o servidor de destino); **HTTP é pull** (cliente puxa). SMTP exige ASCII 7 bits (daí o **MIME** para anexos); HTTP não restringe formato.

### 🟡 Q10. Compare POP3, IMAP e webmail.
**R:** **POP3 (TCP 110):** simples, "baixa e apaga", **sem estado** entre sessões, sem pastas no servidor. **IMAP:** mensagens e **pastas ficam no servidor**, **mantém estado** entre sessões, permite baixar só partes da mensagem (ex.: só cabeçalho) — bom para múltiplos dispositivos. **Webmail:** o navegador acessa via **HTTP** (Gmail), com o servidor fazendo IMAP/SMTP por trás.

### 🟡 Q11. (Cálculo) Dê e interprete as fórmulas do tempo mínimo de distribuição cliente-servidor e P2P.
**R:** **Dcs = max{ N·F/us ; F/dmin }** — o servidor precisa subir N cópias (N·F/us, cresce **linearmente** com N) e o cliente mais lento precisa baixar tudo (F/dmin). **Dp2p = max{ F/us ; F/dmin ; N·F/(us + Σui) }** — o servidor sobe ao menos 1 cópia; o total N·F é servido pela capacidade **agregada** us + Σui, que **cresce com N** → autoescalável. Ex. do slide: F/u = 1 h, us = 10u: com N = 35, cliente-servidor ≈ 3,5 h; P2P fica abaixo de ~1 h.

### 🟡 Q12. O que o HTTP/2 melhora em relação ao HTTP/1.1, e o HTTP/3 em relação ao HTTP/2? 💬 QUIC é protocolo de quê?
**R:** **HTTP/2 (RFC 7540):** multiplexa vários objetos em **frames intercalados numa única conexão TCP**, mitigando o **HOL blocking** (objeto grande não bloqueia mais os pequenos), com priorização e server push. **HTTP/3:** roda sobre o **QUIC** — que é **protocolo de TRANSPORTE** construído sobre UDP (pegadinha!) — com criptografia TLS embutida, handshake de ~**1 RTT** (vs 2–3 do TCP+TLS) e controle de erro/congestionamento **por stream**, eliminando o HOL blocking também no transporte.

### 🟡 Q13. Como funciona o streaming DASH?
**R:** **Dynamic Adaptive Streaming over HTTP**: o vídeo é dividido em **chunks**, cada chunk **codificado em várias taxas/qualidades** no servidor. O cliente consulta um manifesto, **mede a banda disponível** e escolhe, **chunk a chunk**, a qualidade que consegue sustentar — adaptação dinâmica, usando buffer local para absorver variações.

### 🟢 Q14. O que é uma CDN, as duas filosofias de implantação e como o cliente é direcionado ao servidor da CDN?
**R:** Rede de servidores geograficamente distribuídos com cópias do conteúdo (resolve atraso, redundância de tráfego e ponto único de falha). **Enter deep:** muitos clusters pequenos **dentro** das redes de acesso dos ISPs (Akamai). **Bring home:** menos clusters, maiores, em IXPs. O direcionamento é feito **via DNS**: o DNS autoritativo do provedor de conteúdo responde com um **CNAME** apontando para a CDN, cujo DNS escolhe e devolve o IP do melhor cluster; o cliente então baixa direto da CDN. Seleção de cluster: geograficamente mais próximo (nem sempre o melhor caminho de rede) ou por medições de atraso (exige sondas constantes).

### 🟢 Q15. 💬 Em que camada o TLS é implementado e o que ele oferece?
**R:** Embora sirva à segurança "de transporte", o TLS é **implementado na camada de aplicação**, como biblioteca sobre o TCP. Oferece: conexões **criptografadas**, **integridade** dos dados e **autenticação** dos pontos finais.

### 🔴 Q16. Associe: HTTP, FTP (controle/dados), SMTP, POP3, DNS, DHCP — protocolo de transporte e porta.
**R:** HTTP → TCP **80** · FTP controle → TCP **21** / FTP dados → TCP **20** (nova conexão por arquivo — controle "fora da banda") · SMTP → TCP **25** · POP3 → TCP **110** · DNS → **UDP 53** · DHCP → **UDP 67 (servidor) / 68 (cliente)**.

---

## AULA 3 — Transporte, parte 1

### 🔴 Q1. Compare os serviços do TCP e do UDP. 💬 Por que o UDP existe?
**R:** **TCP:** transferência **confiável** e ordenada, orientado à conexão (handshake), **controle de fluxo** e **de congestionamento** — sem garantias de temporização/banda. **UDP:** melhor esforço — sem conexão, sem confiabilidade, sem ordem, sem controle de fluxo/congestionamento. O UDP existe porque: (1) **sem estabelecimento de conexão** (menos RTT inicial); (2) **sem estado** de conexão (servidores atendem mais clientes); (3) **cabeçalho pequeno** (8 bytes vs 20 do TCP); (4) **sem controle de congestionamento** (a aplicação transmite na taxa que quiser). Usos: DNS, streaming, SNMP. 💬 É possível ter confiabilidade sobre UDP? Sim, implementando-a **na aplicação** (ideia do QUIC).

### 🔴 Q2. Como um segmento é demultiplexado para o socket correto no UDP e no TCP?
**R:** **UDP:** o socket é identificado só por **(IP de destino, porta de destino)** — segmentos de origens diferentes com mesmo destino caem no **mesmo** socket. **TCP:** o socket é identificado pela **quádrupla (IP origem, porta origem, IP destino, porta destino)** — o servidor mantém um socket por conexão; dois clientes podem usar a mesma porta de origem (ex.: 9157) e ainda assim cair em sockets distintos, pois o IP de origem difere.

### 🔴 Q3. Compare Go-Back-N e Repetição Seletiva. Descreva o que acontece em cada um quando, com janela 4, o pacote 2 se perde.
**R:**
| | GBN | SR |
|---|---|---|
| ACK | cumulativo | individual |
| Fora de ordem | descarta e reenvia último ACK | armazena em buffer |
| Timers | 1 (mais antigo sem ACK) | 1 por pacote |
| Timeout | retransmite **todos** os não confirmados | retransmite **só** o pacote do timer |

**GBN:** envia 0,1,2(perde),3; receptor entrega 0 e 1, **descarta** 3 e reenvia ack1; remetente envia 4,5 (descartados, ack1 de novo); no timeout do 2, **retransmite 2,3,4,5**. **SR:** receptor entrega 0,1 e **guarda 3,4,5 no buffer** enviando ack3, ack4, ack5; no timeout, o remetente **reenvia só o 2**; ao chegar, o receptor entrega 2,3,4,5 em ordem. SR evita retransmissões desnecessárias ao custo de buffers e um timer por pacote.

### 🟡 Q4. Descreva a evolução rdt 1.0 → 3.0: que problema cada versão resolve e com qual mecanismo.
**R:** **rdt 1.0** (canal perfeito): só empacota/envia e desempacota/entrega — sem feedback. **rdt 2.0** (canal com erros de bits): protocolos **ARQ** — **checksum** para detectar erro + **ACK/NAK** do receptor + **retransmissão**; é pare-e-espere. Falha: e se o próprio ACK/NAK chegar corrompido? Retransmitir gera duplicatas. **rdt 2.1/2.2:** adiciona **número de sequência (0/1)** — o receptor detecta e descarta duplicatas (2.2 elimina o NAK usando ACK duplicado com nº de sequência). **rdt 3.0** (canal com erros **e perdas**): adiciona **temporizador** — no timeout retransmite; duplicatas por retransmissão prematura são tratadas pelo nº de sequência ("protocolo de alternância de bits").

### 🟡 Q5. Como funciona o checksum? 💬 Detectar "nenhum erro" garante integridade?
**R:** O transmissor trata o segmento como sequência de **palavras de 16 bits**, soma todas e coloca no campo checksum o **complemento de 1** da soma. O receptor soma tudo (incluindo o checksum): se der `1111 1111 1111 1111`, nenhum erro detectado; qualquer bit 0 indica erro. **Não garante integridade**: erros em bits diferentes podem se compensar na soma e passar despercebidos.

### 🟡 Q6. (Cálculo) Por que o pare-e-espere é ineficiente? Dê a fórmula da utilização e o efeito do pipelining.
**R:** O emissor transmite por L/R e fica ocioso esperando o ACK por ~RTT. Utilização: **U = (L/R) / (RTT + L/R)**. Ex.: L/R = 0,08 ms e RTT = 30 ms → U ≈ 0,027%. Com **pipelining** (N pacotes no ar): **U = N·(L/R)/(RTT + L/R)** — multiplicada por N até saturar o enlace. Por isso existem GBN e SR.

### 🟢 Q7. Diferencie a comunicação lógica da camada de transporte e da camada de rede.
**R:** Transporte: comunicação lógica **entre processos** (usa portas para entregar ao processo certo); rede: comunicação lógica **entre hospedeiros**. Os protocolos de transporte rodam **apenas nos sistemas finais** — roteadores só implementam até a camada de rede.

---

## AULA 4 — Transporte, parte 2 (TCP)

### 🔴 Q1. Descreva a abertura (3 vias) e o encerramento de uma conexão TCP.
**R:** **Abertura:** (1) cliente envia **SYN** com seu nº de sequência inicial (escolhido aleatoriamente); (2) servidor responde **SYN+ACK** com o seu; (3) cliente envia **ACK** — conexão aberta (esse 3º segmento já pode carregar dados). **Encerramento:** cada lado fecha seu sentido: cliente envia **FIN**, servidor responde ACK; depois o servidor envia seu **FIN**, o cliente responde ACK e espera em **TIME_WAIT** (~30 s, para reenviar o ACK se ele se perder e deixar segmentos velhos expirarem) antes de fechar.

### 🔴 Q2. (Cálculo) Dê as fórmulas de EstimatedRTT, DevRTT e TimeoutInterval. Com EstimatedRTT = 100 ms, DevRTT = 20 ms e uma nova amostra de 140 ms (α = 0,125; β = 0,25), calcule o novo timeout.
**R:** EstimatedRTT = (1−α)·EstimatedRTT + α·SampleRTT; DevRTT = (1−β)·DevRTT + β·|SampleRTT − EstimatedRTT|; **Timeout = EstimatedRTT + 4·DevRTT** (valor inicial: 1 s, RFC 6298).
Cálculo: EstimatedRTT = 0,875·100 + 0,125·140 = **105 ms**; DevRTT = 0,75·20 + 0,25·|140−100| = 15 + 10 = **25 ms**; Timeout = 105 + 4·25 = **205 ms**. (São médias móveis exponenciais: amostras antigas pesam cada vez menos; a margem 4·DevRTT protege contra RTTs oscilantes.)

### 🔴 Q3. Diferencie controle de fluxo e controle de congestionamento no TCP.
**R:** **Controle de fluxo (rwnd):** protege o **receptor** — o receptor anuncia no cabeçalho quanto espaço livre tem no buffer, e o emissor não mantém mais dados não confirmados do que isso (compatibilização de velocidades). **Controle de congestionamento (cwnd):** protege a **rede** — o emissor limita sua taxa conforme o congestionamento percebido (perdas/atrasos). Os dados em trânsito respeitam **min(cwnd, rwnd)**. 💬 Se a janela de recepção chega a **zero**, o emissor continua enviando **sondas de 1 byte** — sem isso, nunca saberia que o buffer liberou espaço.

### 🔴 Q4. Explique as fases do controle de congestionamento do TCP e o que acontece em cada evento de perda. Compare Tahoe e Reno.
**R:** **Slow start:** cwnd começa em 1 MSS e cresce 1 MSS **por ACK** → **dobra a cada RTT** (exponencial, apesar do nome). Ao atingir **ssthresh**, passa a **congestion avoidance:** +1 MSS **por RTT** (linear — parte aditiva do AIMD). Eventos: **timeout** → ssthresh = cwnd/2, **cwnd = 1 MSS**, recomeça slow start; **3 ACKs duplicados** → ssthresh = cwnd/2, janela cai à metade e entra em **fast recovery** (decréscimo multiplicativo). **Tahoe:** trata qualquer perda como timeout (cwnd = 1 sempre). **Reno:** distingue — 3 dup ACKs = metade + fast recovery; só timeout zera. Ex. clássico: ssthresh = 8; cresce 1,2,4,8 depois linear até 12; perda por 3 dup ACKs → novo ssthresh = 6; Tahoe volta a 1, Reno continua de ~6 linearmente. Resultado: o "dente de serra" do TCP (sonda a banda até perder, reduz, volta a crescer). Taxa média ≈ **cwnd/RTT**.

### 🟡 Q5. Como funcionam os números de sequência e de reconhecimento do TCP? Dê o exemplo do Telnet.
**R:** **Seq** = número do **primeiro byte** de dados do segmento (contagem por bytes, não por segmentos); **ACK** = número do **próximo byte esperado**, e é **cumulativo** (confirma tudo até ele). Exemplo (eco de caractere): A envia Seq=42, ACK=79, dados='C' → B ecoa com Seq=79, **ACK=43** ('recebi o 42, espero o 43'; o ACK pega carona no segmento de dados — piggybacking) → A responde Seq=43, ACK=80. Segmentos fora de ordem: o receptor guarda no buffer e envia **ACK duplicado** apontando a lacuna.

### 🟡 Q6. O que é retransmissão rápida (fast retransmit) e por que ela existe?
**R:** O timeout costuma ser longo → recuperar perda só por ele atrasa muito. Quando um segmento se perde mas os seguintes chegam, cada um gera um **ACK duplicado** apontando a lacuna. Ao receber **3 ACKs duplicados** (4 ACKs iguais), o emissor **retransmite imediatamente** o segmento pendente de menor seq, **sem esperar o timeout**.

### 🟡 Q7. 💬 A conexão TCP é um exemplo de comutação de circuitos?
**R:** **Não.** A "conexão" é **lógica**, existe apenas no estado dos dois sistemas finais. Os roteadores intermediários **não sabem** da conexão e **nenhum recurso é reservado** no caminho — a rede continua sendo de comutação de pacotes com best effort.

### 🟡 Q8. O que é o QUIC e por que ele representa a "evolução" da camada de transporte?
**R:** **QUIC (RFC 9000)** é um protocolo de **transporte** construído **sobre o UDP** (implementável na aplicação, evolui sem mudar o kernel/roteadores). É orientado à conexão e seguro: **incorpora a criptografia do TLS** no próprio handshake (~1 RTT para conexão + segurança), transporta **múltiplos streams** independentes (sem HOL blocking), tem transferência confiável e controle de congestionamento **amigável ao TCP**. É a base do **HTTP/3**; em 2025 estimava-se ~40% do tráfego da Internet.

### 🟢 Q9. Explique brevemente: CUBIC, ECN, TCP Vegas e o problema de equidade com UDP.
**R:** **CUBIC:** variante padrão do Linux; após a perda cresce rápido em direção a **W_max** (janela da última perda) e desacelera perto dele (função cúbica) — usa melhor o enlace que o Reno. **ECN:** controle **assistido pela rede** — roteadores marcam bits no cabeçalho quando começam a congestionar; detecta congestionamento **antes de haver perda**, sem pacotes extras. **Vegas:** controle **baseado em atraso** — se o RTT cresce (filas se formando), reduz a taxa antes de perder ("manter o cano cheio, mas não mais do que cheio"). **Equidade:** o AIMD tende a dividir a banda igualmente (R/K por conexão), mas o **UDP não reduz a taxa** (atropela o TCP) e aplicações podem abrir **múltiplas conexões TCP paralelas** para capturar mais banda.

---

## AULA 5 — Rede: plano de dados

### 🔴 Q1. Diferencie repasse (forwarding) e roteamento (routing).
**R:** **Repasse:** levar o pacote do enlace de **entrada** ao enlace de **saída** correto dentro de **um** roteador, consultando a tabela de repasse — função **local**, do **plano de dados**, implementada em **hardware** (nanossegundos). **Roteamento:** determinar o **caminho fim a fim** da origem ao destino, via algoritmos de roteamento — função **global**, do **plano de controle**, em **software** (ms/s). O algoritmo de roteamento calcula a tabela que o repasse consulta.

### 🔴 Q2. (Cálculo) Explique a regra do prefixo mais longo e aplique: tabela com prefixos `11001000 00010111 00010***` → if 0; `11001000 00010111 00011000` → if 1; `11001000 00010111 00011***` → if 2; caso contrário → if 3. Para onde vai `11001000 00010111 00011000 10101010`?
**R:** O roteador compara o endereço de destino com todos os prefixos da tabela; entre os que **casam**, vence o de **maior número de bits fixos** (mais específico). O endereço dado casa com a linha 2 (`00011000`, 24 bits) e com a linha 3 (`00011***`, 21 bits) → vence a mais específica: **interface 1**. (Já `...00010110 10100001` casa só com a 1ª linha → interface 0.)

### 🔴 Q3. Descreva as 4 mensagens do DHCP e o que mais ele fornece além do IP.
**R:** (1) **DHCP discover** — cliente, em broadcast (origem 0.0.0.0:68 → 255.255.255.255:67): "há servidor DHCP?"; (2) **DHCP offer** — servidor oferece um IP (`yiaddr`) com prazo de concessão (lease); (3) **DHCP request** — cliente aceita o IP oferecido; (4) **DHCP ACK** — servidor confirma. Portas fixas: servidor **UDP 67**, cliente **UDP 68**. Também fornece **máscara de sub-rede, gateway padrão e servidor DNS**; é plug-and-play, e as 2 primeiras etapas podem ser puladas para renovar IP anterior.

### 🔴 Q4. Explique o funcionamento do NAT, incluindo a tabela de tradução, e cite as faixas privadas.
**R:** Faixas privadas (**RFC 1918**): **10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16** — não roteáveis na Internet. Saída: o roteador NAT troca **(IP privado, porta)** de origem por **(IP público, nova porta)** e registra o mapeamento na **tabela NAT** (ex.: `10.0.0.1:3345 ↔ 138.76.29.7:5001`). Volta: a resposta chega ao (IP público, porta), o NAT consulta a tabela e reescreve o destino de volta para `10.0.0.1:3345`. Assim muitos hosts privados compartilham **um** IP público, multiplexados por porta.

### 🔴 Q5. Cite as principais diferenças do IPv6 em relação ao IPv4 e as estratégias de transição.
**R:** IPv6: endereços de **128 bits** (vs 32), cabeçalho **fixo de 40 bytes** simplificado, **sem fragmentação nos roteadores** (só na origem), **sem checksum de cabeçalho** (menos processamento por salto), novo tipo **anycast**, campos de **rótulo de fluxo**/classe de tráfego, e **ICMPv6** (incorpora o IGMP). Notação: 8 grupos hex de 16 bits, com compressão de zeros `::`. Transição (roteadores antigos não entendem IPv6): **pilha dupla** (nós falam v4 e v6) e **tunelamento** (datagrama IPv6 dentro de um IPv4 para atravessar trechos só-v4). Motivação: IPv4 esgotado na IANA/ICANN desde 2011.

### 🟡 Q6. Explique a fragmentação IPv4: por que existe, quais campos a suportam e onde ocorre a remontagem.
**R:** Enlaces têm **MTUs** diferentes (Ethernet = **1500 bytes**); um datagrama maior que a MTU do próximo enlace é dividido em fragmentos, cada um virando um datagrama. Campos: **identificação** (igual em todos os fragmentos do mesmo datagrama), **flag** "mais fragmentos" e **deslocamento (offset)** (posição dos dados no original). A **remontagem ocorre apenas no destino final** — roteadores intermediários não remontam.

### 🟡 Q7. Descreva os 4 componentes de um roteador e os 3 tipos de elemento de comutação.
**R:** (1) **Portas de entrada** (terminação de linha → desencapsulamento → lookup/fila, com cópia local da tabela); (2) **elemento de comutação**; (3) **portas de saída** (fila/buffer → encapsulamento → linha); (4) **processador de roteamento** (plano de controle: calcula as tabelas e as envia às portas). Fabrics: **memória** (via CPU, 1ª geração, mais lento) < **barramento** compartilhado (limitado pela banda do bus) < **crossbar** (malha com transferências em paralelo, mais rápido). Repasse em hardware/ns; roteamento em software/ms.

### 🟡 Q8. Onde se formam filas num roteador e o que é o bloqueio HOL?
**R:** **Na saída:** quando vários pacotes chegam "ao mesmo tempo" para a mesma porta (contenção) — buffer estoura → perda. **Na entrada:** quando o elemento de comutação é mais lento que as chegadas. **HOL (head-of-line) blocking:** o pacote na frente da fila de entrada, esperando uma saída ocupada, **bloqueia os de trás** mesmo que as saídas deles estejam livres — com ~58% de carga a degradação já é considerável. Dimensionamento de buffer: regra clássica **RTT×C** (ex.: 0,25 s × 10 Gb/s = 2,5 Gb); recomendação atual **RTT×C/√N** para N fluxos. Descarte: tail drop ou AQM/**RED**. Escalonamento: FIFO, filas de **prioridade**, **WFQ** (round-robin ponderado por pesos).

### 🟡 Q9. O que é o repasse generalizado (match+action) e o que o OpenFlow permite casar e fazer?
**R:** Generaliza a tabela de repasse: em vez de olhar só o IP de destino, cada entrada tem **campos de combinação + contadores + ações**. O **OpenFlow** casa campos das camadas 1–4 (porta de ingresso; MACs origem/destino, tipo Ethernet, VLAN; IPs origem/destino, protocolo; portas TCP/UDP) e executa ações: **encaminhar** para porta(s), **descartar**, **modificar campos**, ou **encapsular e enviar ao controlador**. Com isso, um mesmo dispositivo faz papel de roteador, switch, firewall e NAT.

### 🟢 Q10. O que é o modelo de serviço best effort da Internet? E uma rede de circuitos virtuais?
**R:** **Best effort:** a camada de rede da Internet **não garante nada** — nem banda, nem ausência de perda, nem ordem, nem atraso máximo. **Circuitos virtuais** (ATM, frame relay): serviço orientado à conexão na camada de rede — caminho estabelecido antes dos dados, **número de CV por enlace** (muda a cada salto), **estado de conexão nos roteadores**, 3 fases (estabelecimento, transferência, encerramento). CV vem da telefonia (complexidade no núcleo); datagrama vem da computação (complexidade na borda, núcleo simples).

### 🟢 Q11. Contar sub-redes: numa topologia com 3 roteadores interligados dois a dois e 3 redes de hosts, quantas sub-redes há?
**R:** **6**: as 3 sub-redes de hosts (ex.: 223.1.1.0/24, 223.1.2.0/24, 223.1.3.0/24) **mais** os 3 enlaces ponto a ponto entre roteadores — **cada enlace entre roteadores também é uma sub-rede**. Método: destaque cada interface; cada "ilha" de rede isolada é uma sub-rede. Lembre: o endereço IP pertence à **interface**, não ao hospedeiro.

### 🟢 Q12. O que são middleboxes? Dê exemplos e como podem ser implementados hoje.
**R:** Qualquer equipamento intermediário que realiza funções **além das de um roteador IP padrão** no caminho dos dados. Exemplos: **NAT**, **firewall/IDS/IPS**, **caches/otimizadores**. Hoje podem ser implementados por **NFV** (funções de rede virtualizadas em hardware genérico) ou como serviços em **nuvem**.

---

## AULA 6 — Rede: plano de controle

### 🔴 Q1. Compare os algoritmos de estado de enlace (LS) e vetor de distância (DV). Quais protocolos usam cada um?
**R:** **LS (Dijkstra):** algoritmo **global** — por difusão de estado de enlace, todos os nós obtêm a topologia e os custos **completos** e idênticos, e cada um roda Dijkstra localmente; complexidade **O(n²)**; usado pelo **OSPF**. **DV (Bellman-Ford):** **descentralizado, iterativo e assíncrono** — cada nó conhece só os custos até os vizinhos e os vetores que eles enviam; recalcula por **D_x(y) = min_v{c(x,v) + D_v(y)}** e, se seu vetor mudar, notifica os vizinhos; usado pelo **RIP**. No DV, "más notícias" se propagam devagar (contagem ao infinito — por isso o RIP limita a **15 saltos**).

### 🔴 Q2. (Cálculo) No grafo u,v,w,x,y,z com custos u–v=2, u–x=1, u–w=5, v–x=2, v–w=3, x–w=3, x–y=1, w–y=1, w–z=5, y–z=2, encontre o menor caminho de u a z.
**R:** Executando Dijkstra a partir de u: D(x)=1 (via u–x); D(v)=2; D(y)=D(x)+1=2 (u→x→y); D(w)=min(5, 2+3, 1+3, 2+1)=3 (u→x→y→w); D(z)=min(D(w)+5, D(y)+2)=min(8, 4)=**4**. Menor caminho: **u → x → y → z, custo 4**. (Saiba montar a tabelinha de iterações: a cada passo, adicione o nó de menor D ao conjunto N' e relaxe os vizinhos: D(v) = min(D(v), D(w)+c(w,v)).)

### 🔴 Q3. O que é um Sistema Autônomo (AS)? Por que o roteamento intra-AS e inter-AS usam protocolos diferentes?
**R:** AS = grupo de roteadores sob o **mesmo controle administrativo**, identificado por um **ASN** globalmente único (ICANN). Protocolos diferentes por: (1) **política** — entre ASs há acordos comerciais e restrições de trânsito (um AS pode recusar tráfego de terceiros); dentro de um AS, dono único, política irrelevante; (2) **escalabilidade** — a hierarquia reduz tabelas e tráfego de atualização (impossível difundir estado de milhões de roteadores); (3) **desempenho** — intra-AS pode otimizar custo/desempenho; inter-AS, a política domina.

### 🔴 Q4. Explique o BGP: sessões eBGP/iBGP, atributos AS-PATH e NEXT-HOP, e a ordem de seleção de rotas.
**R:** O **BGP** é o protocolo inter-AS universal da Internet ("o mais importante da Internet"), descentralizado e assíncrono; roteia por **prefixos** e roda sobre **TCP**. **eBGP:** sessões entre roteadores de **ASs diferentes** (aprender rotas de fora); **iBGP:** entre roteadores do **mesmo AS** (propagar internamente); roteadores de borda rodam ambos. Rota = prefixo + atributos: **AS-PATH** (lista de ASs percorridos — detecta e evita laços: AS que vê seu próprio ASN descarta) e **NEXT-HOP** (IP do roteador de entrada da rota). **Seleção (ordem de desempate):** 1º **preferência local** (política do administrador); 2º **AS-PATH mais curto**; 3º **NEXT-HOP mais próximo** (= roteamento **batata quente**: sair do próprio AS pelo caminho interno mais barato — algoritmo "egoísta"); 4º identificadores BGP.

### 🟡 Q5. Compare o controle por roteador e o controle logicamente centralizado (SDN). "Logicamente centralizado" significa fisicamente centralizado?
**R:** **Por roteador (tradicional):** cada roteador roda o algoritmo localmente e troca mensagens com os demais; repasse e roteamento dentro do mesmo equipamento. **SDN:** um **controlador remoto** calcula as tabelas e as instala nos dispositivos via um **agente de controle (CA)** de funcionalidade mínima, que não fala com outros CAs nem calcula rotas. **Não** — logicamente centralizado significa uma visão única de serviço; na prática o controlador é **replicado/distribuído** por desempenho e tolerância a falhas (pegadinha). Casos reais: **Google B4** e **Microsoft SWAN** (2013).

### 🟡 Q6. Cite as 4 características-chave da SDN e explique a arquitetura (northbound/southbound).
**R:** (1) Repasse **baseado em fluxo** (match+action); (2) **separação** entre plano de dados e de controle; (3) funções de controle **externas** aos comutadores; (4) rede **programável**. Arquitetura em 3 camadas: **aplicações de controle** (roteamento, controle de acesso, balanceamento) ↔ **northbound API** ↔ **controlador SDN** (network OS, estado da rede) ↔ **southbound API (OpenFlow, TCP 6653)** ↔ comutadores. Mensagens OpenFlow: controlador→switch (configuração, modificar/ler estado, packet-out); switch→controlador (**packet-in**, estado de porta, fluxo removido). Ex. de operação: enlace cai → switch notifica → controlador atualiza grafo → aplicação Dijkstra recalcula → novas entradas de fluxo instaladas.

### 🟡 Q7. O que é o ICMP e como o traceroute o utiliza? 💬 Como o traceroute sabe quando parar?
**R:** **ICMP** = protocolo de mensagens de controle da Internet, usado entre hosts e roteadores para relatar erros e sinalização; suas mensagens viajam como **payload de datagramas IP** (formato: tipo, código, checksum + cabeçalho IP e 8 primeiros bytes do datagrama causador). Principais: **8/0** pedido de eco (ping), **0/0** resposta de eco, **11/0** TTL expirado, **3/3** porta inalcançável. **Traceroute:** envia segmentos UDP com **TTL = 1, 2, 3…** para uma **porta improvável**; o n-ésimo roteador zera o TTL, descarta e devolve **ICMP 11/0** (revelando-se); quando o datagrama finalmente chega ao **destino**, este devolve **ICMP 3/3 (porta inalcançável)** — o sinal de parada.

### 🟡 Q8. RIP × OSPF: classifique cada um e aponte as diferenças.
**R:** **RIP:** protocolo intra-AS de **vetor de distância**; métrica em saltos, limitado a **15 saltos** (16 = infinito); **convergência lenta**. **OSPF:** intra-AS de **estado de enlace**; cada roteador difunde os estados dos seus enlaces (periodicamente e quando há mudança) e roda **Dijkstra**; **custos definidos pelo administrador** (permite engenharia de tráfego).

### 🟢 Q9. O que é IP-Anycast, quem o usa bastante e por que as CDNs o evitam?
**R:** Anunciar via BGP **o mesmo prefixo a partir de vários pontos** da Internet — o roteamento leva cada cliente ao ponto "mais próximo". Muito usado pelos **servidores DNS** (raiz) — DNS usa UDP, sem estado. **CDNs evitam** porque uma mudança de rota no meio de uma **conexão TCP** pode mandar pacotes da mesma conexão para réplicas diferentes e quebrá-la.

### 🟢 Q10. Descreva a estrutura de gerenciamento de rede e os dois modos do SNMP.
**R:** 5 componentes: **entidade gerenciadora** (aplicação no NOC), **dispositivos gerenciados**, **dados** (MIB), **agente de gerenciamento** (processo no dispositivo) e **protocolo de gerenciamento**. SNMP opera em: **requisição/resposta** (o gerente consulta, o agente responde — polling) e **trap** (o agente notifica **espontaneamente** um evento, ex.: interface caiu). Alternativas: CLI e NETCONF/YANG.

---

## SIMULADO RELÂMPAGO (responda de memória, 1 linha cada)

1. QUIC é protocolo de que camada? → **Transporte** (sobre UDP).
2. TLS é implementado em que camada? → **Aplicação**.
3. 3 ACKs duplicados no TCP disparam o quê? → **Fast retransmit** (+ fast recovery no Reno).
4. Timeout no TCP: cwnd vira quanto? → **1 MSS** (ssthresh = cwnd/2).
5. Quem protege o receptor? E a rede? → **rwnd** (fluxo); **cwnd** (congestionamento).
6. Demux UDP usa quantos campos? E TCP? → **2** (IP+porta destino); **4** (quádrupla).
7. Consulta DNS em que o local faz todo o trabalho? → **Iterativa**.
8. Registro DNS de apelido? De correio? → **CNAME**; **MX**.
9. Ordem do DHCP? → **Discover, Offer, Request, ACK** (UDP 67/68).
10. Faixas privadas do NAT? → **10/8, 172.16/12, 192.168/16**.
11. MTU da Ethernet? Onde remonta fragmento? → **1500 bytes**; **no destino final**.
12. IPv6: tamanho do endereço e do cabeçalho? → **128 bits**; **40 bytes fixos**.
13. Empate de prefixos na tabela de repasse: quem vence? → **O prefixo mais longo**.
14. OSPF usa qual algoritmo? RIP? → **Dijkstra/LS**; **Bellman-Ford/DV** (15 saltos).
15. Ordem de seleção BGP? → **Pref. local → AS-PATH curto → NEXT-HOP próximo (batata quente) → IDs**.
16. eBGP e iBGP rodam sobre o quê? → **TCP**.
17. Traceroute para quando recebe qual ICMP? → **Tipo 3, código 3** (porta inalcançável).
18. Ping usa quais tipos ICMP? → **8/0** (pedido) e **0/0** (resposta).
19. Fórmula do timeout TCP? → **EstimatedRTT + 4·DevRTT**.
20. Tempo HTTP não persistente (1 objeto)? → **2·RTT + F/R**.
21. Atraso nodal = ? → **d_proc + d_fila + d_trans + d_prop**.
22. Vazão fim a fim = ? → **min dos enlaces do caminho**.
23. PDUs das 4 camadas de cima? → **Mensagem, segmento, datagrama, quadro**.
24. Switch processa até qual camada? Roteador? → **Enlace**; **rede**.
25. GBN: o que faz no timeout? SR? → **Retransmite todos os não confirmados**; **só o pacote do timer**.
