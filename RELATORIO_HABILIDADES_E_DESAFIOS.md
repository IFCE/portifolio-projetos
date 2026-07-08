# Relatório de Habilidades Técnicas e Desafios de Engenharia

> Documento de portfólio profissional. Consolida os principais **desafios técnicos** enfrentados e as **stacks e abordagens** adotadas para resolvê-los, a partir da varredura do acervo pessoal de projetos dos últimos anos.
>
> **Nota de confidencialidade:** todos os projetos foram anonimizados. Nomes de empresas, produtos, clientes, órgãos e identificadores foram removidos — mantêm-se apenas o **domínio do problema**, os **desafios de engenharia** e as **soluções técnicas**.
>
> **Sincronizado com os repositórios em 2026-07-08** (via `git pull` em todos os projetos com remoto acessível).

---

## 1. Perfil em uma frase

Engenheiro de software **full-stack e cloud**, com maior profundidade em **backend distribuído (Java/Spring, Quarkus, Go, Node/TypeScript, Python)** e forte atuação em **arquiteturas event-driven, serverless (AWS) e multi-tenant**, integração de **sistemas legados**, **IoT** e, na fronteira mais recente, **plataformas de IA/LLM** (agentes, RAG, ML clássico e MLOps). Experiência comprovada de ponta a ponta: modelagem de domínio, API, mensageria, banco, frontend (Angular/React), mobile (Flutter, React Native, Kotlin Multiplatform) e infraestrutura como código.

---

## 2. Panorama quantitativo do acervo

| Métrica | Valor |
|---|---|
| Módulos Maven (Java: Spring Boot / Spring Cloud / Quarkus) | **81** |
| Projetos Node/JavaScript/TypeScript (`package.json`) | **177** |
| Projetos Python (`requirements.txt` / `pyproject.toml`) | **41** |
| Microserviços Go (`go.mod`) | **8** |
| Dockerfiles / arquivos docker-compose | **130 / 29** |
| Linguagens de produção usadas | Java, Kotlin, TypeScript/JavaScript, Python, Go, Rust, C#/.NET, Dart, Delphi (legado), C++, Bash |

O acervo cobre desde **produtos em produção** e **sistemas críticos multi-tenant** até **PoCs de arquitetura**, **simuladores/testes de carga** e **propostas de P&D em IA** de concepção madura.

---

## 3. Stack técnica dominada

**Backend & Frameworks**
- **Java 8 → 21** com **Spring Boot 1.5 → 4.0**, **Spring Cloud (Netflix OSS: Eureka, Zuul, Feign, Hystrix, Ribbon, Sleuth/Zipkin)**, Spring Security, Spring Data JPA, WebFlux.
- **Quarkus 2.16 → 3.11** com **native image (GraalVM/Mandrel)** para AWS Lambda (otimização de cold start).
- **Java EE 8 / Jakarta** (CDI, JAX-RS, JTA) em **WildFly**.
- **Go 1.24/1.25** (Gin, pgx/v5, sqlc, goose) em clean architecture.
- **Node/TypeScript**: Fastify, Express, NestJS-style, Remix, Next.js 15, Deno (Edge Functions).
- **Python**: FastAPI + Pydantic v2, SQLAlchemy async, Django/DRF, Celery, APScheduler.
- **Rust** (Tauri v2 desktop, worker Tokio), **Kotlin/.NET** (legado e mobile).

**Frontend & Mobile**
- **Angular 5 → 21** (standalone, signals, zoneless, RxJS, Tailwind 4, PrimeNG, interceptors funcionais).
- **React 18/19**, Next.js 15 (App Router, RSC, BFF), Vite, Remix, Radix/shadcn.
- **Mobile**: Flutter/Dart, React Native/Expo, **Kotlin Multiplatform + Compose**, Android nativo (Jetpack Compose, kiosk/DevicePolicyManager).

**Dados & Mensageria**
- PostgreSQL (multi-tenant por schema/filtro, `pgvector`, `unaccent`, `pglogical`, window functions, Flyway/goose — dezenas a >170 migrations por projeto), MariaDB, SQL Server, DynamoDB, MongoDB, Redis, H2/LibSQL, ChromaDB/FAISS.
- **Kafka** (mTLS PKCS12, Kafka Connect, **Debezium CDC**), **RabbitMQ (AMQP)**, **AWS SQS/SNS**, Redis pub/sub, STOMP/WebSocket, SSE.

**Cloud & Infra**
- **AWS**: Lambda (HTTP/REST/event triggers), API Gateway, Cognito, DynamoDB, SQS/SNS, S3, Secrets Manager, IoT Core, Aurora Serverless, STS, ECS/Fargate, CloudFront, RDS, VPC.
- **IaC**: AWS CDK, Serverless Framework, **Terraform**, CloudFormation/SAM.
- **Containers/Orquestração**: Docker, **Kubernetes** (Kustomize, HPA/PDB), **Linkerd** service mesh, LocalStack, Railway.
- **Armazenamento de objetos**: AWS S3, **Cloudflare R2**, Tigris, MinIO (URLs pré-assinadas, organização por data).
- **Observabilidade**: Prometheus, Grafana/Loki, OpenTelemetry, Sentry, New Relic, Nagios/Zabbix.
- **Auth/Identidade**: **AWS Cognito** e **Keycloak** (OAuth2/OIDC Resource Server, JWT, RBAC), mTLS X.509, chaves STS efêmeras.

**IA / LLM / ML**
- Agentes: **Mastra**, **LangChain/LangGraph**, **MCP (Model Context Protocol)** como cliente e servidor, Vercel AI SDK, tool-calling, LLM-as-judge, RAG.
- LLMs: Anthropic Claude, OpenAI, OpenRouter, **modelos locais via Ollama/vLLM** (Qwen, Llama, DeepSeek-Coder).
- ML/DS: scikit-style com **Smile**, GANs (TensorFlow/Keras), PyTorch/Lightning, DINOv2, sentence-transformers, cross-encoder reranking, QLoRA, pgvector/ChromaDB/FAISS, Apache Commons Math (estatística implementada do zero).

**Qualidade & Governança**
- Testcontainers, **ArchUnit**, JUnit/Mockito, Vitest, Playwright, pytest (mypy strict, cobertura ≥80%), Spring REST Docs, Schemathesis.
- Cultura de **ADRs**, skills/playbooks codificados, auditorias de segurança OWASP formais, gates métricos em benchmarks.

---

## 4. Principais desafios técnicos e como foram resolvidos

Esta é a seção central: cada item é um **problema real** enfrentado e a **abordagem técnica** aplicada.

### 4.1 Multi-tenancy (implementado de 5 formas distintas, conscientemente escolhidas)

O isolamento de dados entre clientes é um fio condutor do acervo, resolvido de acordo com o trade-off de cada sistema:

| Estratégia | Como foi feito | Quando aplicada |
|---|---|---|
| **Schema-per-tenant** (Hibernate) | `TenantContext` ThreadLocal + `SchemaMultiTenantConnectionProvider` com `SET SCHEMA`/`search_path`; **fail-closed** (query sem tenant lança exceção, nunca toca o schema público) | ERPs e núcleos transacionais |
| **Coluna + filtro** | Hibernate `@Filter` por `company_id`/`fabrica_id`; herança `BaseEntity → TenantEntity` | SaaS shared-schema |
| **Database-per-tenant** | Resolver de tenant a partir do path (Vert.x `RoutingContext`) no Quarkus | Migração serverless |
| **Fila SQS dedicada por tenant** | Nome de fila derivado do `dominio` extraído do **claim JWT** | Integrações event-driven |
| **Stack/distribuição AWS por cliente** | `sls deploy --stage <cliente>`, isolamento físico total | Clientes de compliance elevado |

**Lição de segurança aplicada:** evolução visível de derivar o tenant de um **header não autenticado** (falha reconhecida em PoCs) para derivá-lo do **claim JWT assinado**, com validação fail-closed nos produtos maduros.

**Incidente de produção resolvido:** contaminação de tenant entre threads (6–34% de mensagens gravadas no schema errado sobre ~41M chamadas AMQP), causada por `InheritableThreadLocal` + dois `ThreadLocal` dessincronizados num consumer sem isolamento. **Solução:** `ThreadLocal` único com padrão save/finally-restore e um wrapper `runWithTenant`, validado a 0% de vazamento.

### 4.2 Arquiteturas event-driven e serverless (AWS)

- **Correlação topológica de causa-raiz** (telecom): transformar rajadas de milhares de alarmes de equipamentos em um único alarme de causa-raiz no componente físico a montante. Modelagem da planta como **árvores por tipo de componente**, **janela temporal de eventos**, tabelas de threshold, concorrência com `CopyOnWriteArrayList` + locker dedicado e processamento multithread. Ingestão via **Kafka** (`@KafkaListener`, produtores dedicados por tipo de evento) com caminho alternativo via **SNS → REST**.
- **Consumo pull-based para contornar NAT/firewall:** clientes on-premise atrás de firewall consomem eventos por **long polling SQS** — sem VPN e sem chaves AWS fixas na máquina do cliente.
- **Zero-trust com credenciais efêmeras:** emissão de credenciais **STS de curta duração (1h)** escopadas a **uma única fila** via `AssumeRole` com *session policy* inline, auditáveis por `RoleSessionName` no CloudTrail.
- **Idempotência e entrega confiável:** upsert via `MERGE ... WITH (ROWLOCK)`, retry de deadlock (SQL Server erro 1205) com backoff exponencial, DLQ, `on conflict do nothing`, **transactional outbox** (evento de domínio na mesma transação da mutação, despachado por worker).
- **Camadas anti-corrupção** para reconciliar contratos de dados entre ERP e marketplaces/integrações externas.

### 4.3 Otimização de cold start com GraalVM native image

Migração de monólitos Spring on-premise para **Quarkus native + AWS Lambda**. Domínio das armadilhas do native image: `@RegisterForReflection`, contorno da inicialização build-time de `java.util.Random` (`--initialize-at-run-time`, `--trace-object-instantiation`), afinação de clientes HTTP por tipo (sync `url-connection-client` vs async `netty-nio-client`), deploy via **AWS SAM** (`provided.al2`, autorizador JWT do API Gateway validando Cognito).

### 4.4 Integração com sistemas legados (tema forte e recorrente)

- **Bridge/BFF sobre sistema legado sem API:** quando a API REST "oficial" nunca foi entregue, construção de um **BFF (Next.js 15)** que faz ponte com um legado PHP + SPA Vue em produção — replicando o handshake de sessão (captura de `PHPSESSID`, captcha por imagem, troca sessão→JWT), mimetizando o SPA (checagem de Origin/Referer) e fazendo **engenharia reversa de HTML/jqGrid** sem documentação.
- **Interop criptográfico com Delphi:** reimplementação em **Rust** do algoritmo **DCPcrypt Blowfish-CFB** para ler credenciais de arquivo `.INI` de um ERP legado, mantendo compatibilidade binária com a DLL original.
- **Sincronização cloud ↔ on-premise (SQL Server):** cliente desktop **Tauri v2 / Rust** (~2000 linhas) com driver SQL Server puro-Rust (`tiberius`) e **cliente SQS + assinatura SigV4 implementados à mão** (sem SDK); UPSERT idempotente, renovação dupla de credenciais STS, auto-update via `minisign`.
- **Sincronização incremental por diff:** ponte CSV↔cloud com **hash SHA-256 do arquivo** → diff por chave natural → envio só das linhas alteradas; handshake por **arquivo-sinal** + rename atômico; mapeamento coluna→campo externalizado em YAML (editável sem recompilar).
- **Reengenharia** de sistemas Delphi/.NET/Xamarin para Java/Angular/Kotlin, com o legado servindo de "fonte da verdade" do contrato de dados.

### 4.5 Ingestão de dados de alto volume e fontes instáveis

- **ETL massivo resiliente:** importação de base nacional (arquivo delimitado com **94 colunas**) em **batches de 4000**, pool fixo de 8 threads, `JdbcTemplate.batchUpdate`, retry por lote e `INSERT` montado por reflection sobre anotações JPA; **lock de importação** para impedir cargas concorrentes.
- **Web scraping de portais governamentais sem API:** scraping assíncrono paralelo (`asyncio`/`aiohttp` + BeautifulSoup), **engenharia reversa de URLs** (re-encoding Base64/ISO-8859-1↔UTF-8), parsing frágil de tabelas HTML com normalização de acentos.
- **Ingestão automatizada por e-mail (IMAP):** tarefa agendada que conecta via `imaps`, busca mensagens por data e faz **descida recursiva na árvore MIME** (inclusive `message/rfc822` aninhado) para extrair anexos de planilhas em formatos variados.
- **Enriquecimento externo com tolerância a rate limiting:** consulta a APIs públicas (CNPJ, CEP/geo) com left-pad, retry e loop de resiliência.
- **Streaming de arquivos gigantes:** parsing de JSON de 2GB+ com **`ijson`** mantendo ~100MB de RAM; anonimização estrutural LGPD (PII → dados falsos plausíveis preservando o schema).

### 4.6 Domínio de dados / algoritmos / CS aplicada

- **Modelagem de rede como grafo dirigido** (JGraphT): regras de fan-in/fan-out por tipo de equipamento impostas por **Observer** no momento da inserção da aresta; **Dijkstra** (menor caminho), **DFS iterativo** para calcular a subárvore afetada — o *blast radius* (clientes impactados numa falha). Decisão de modelagem: fibras como **vértices**, não arestas.
- **Biblioteca estatística própria** sobre Apache Commons Math: regressão linear, **Kendall's Tau** (tendência monotônica) implementado à mão, média móvel, **suavização exponencial** e **bootstrap** (intervalo de confiança por reamostragem) para análise preditiva de séries temporais.
- **Motor de risco** por séries temporais (score de evasão/risco com tendência QUEDA_FORTE/RECUPERAÇÃO e classificação em faixas).
- **Cálculos de engenharia física em código:** níveis acústicos logarítmicos (LAeq), atenuação, extração de geometria a partir de CAD/DXF; conversão física de sinal de sensor (4–20 mA → bar).

### 4.7 Correção sob concorrência e consistência distribuída

- **Consistência com hardware físico:** autorizar/parar bombas de combustível via vendor de pista, com **WAL de reservas**, **goroutine reaper** de reservas órfãs após crash de pod, **preempção "vendor-as-truth"** e kill-switch.
- **Concorrência financeira:** `SELECT FOR UPDATE` com **ordem fixa de lock** (previne deadlock), merge conservador transacional (só preenche colunas nulas, nunca sobrescreve dado bom com pior), idempotência de webhook com claim/rollback.
- **Invariantes impostas no banco, não só na aplicação:** triggers append-only (consentimentos, pagamentos, ledger de estoque), RLS com **CI gate** que bloqueia deploy se alguma tabela ficar sem Row-Level Security.

### 4.8 Realtime e streaming

- **Realtime via AWS IoT Core (MQTT)** em vez de WebSocket próprio: publicação em tópicos por usuário (QoS 1), publisher plugável por `@ConditionalOnProperty`, frontend conectando ao endpoint IoT com assinatura SigV4 via Cognito Identity Pool.
- **SSE com escala horizontal:** fan-out via **Redis pub/sub** com *pattern subscribe* (`PSubscribe`) para múltiplas instâncias.
- STOMP/WebSocket sobre RabbitMQ (inclusive **implementação manual de frames STOMP sobre Ktor WebSocket** em Kotlin Multiplatform).

### 4.9 IoT edge-to-cloud

- Telemetria **ESP32 → MQTT/TLS X.509 → AWS IoT Core → IoT Rules → DynamoDB**; conversão física precisa de sensor de pressão; robustez de campo (reconexão MQTT/TLS).
- **Provisionamento de fleet:** serviço que cria Things/certificados/policies no IoT Core; **persistência híbrida SQL (Postgres) + NoSQL (DynamoDB)** com paginação por cursor sobre série temporal.
- **Autenticação dupla:** JWT Cognito para humanos + API key de dispositivo (`ROLE_DEVICE`) para hardware.
- **Embarcado sem driver pronto:** leitura **bit-banging** de sensor DHT22 direto via GPIO (decodificação manual dos 40 bits por temporização de pulso + checksum), filtro de média móvel com rejeição de outliers, arquitetura multi-thread.

### 4.10 Segurança aplicada e compliance

- OAuth2/OIDC Resource Server (Cognito/Keycloak), RBAC (`cognito:groups → ROLE_*`, `@PreAuthorize`), erros padronizados **RFC 7807**.
- **Criptografia em repouso:** AES-256-GCM (IV 12 bytes + tag 128 bits) para certificados fiscais `.pfx`, CSC e senhas; assinatura digital **XML-DSig / PKCS#12**; TrustStore combinando `cacerts` da JVM com cadeia de certificados de órgão emissor, fixando TLSv1.2.
- **Auditorias OWASP formais** com achados reais documentados (ex.: **TOCTOU / CWE-367** em filtro de tenant baseado em ThreadLocal).
- **LGPD by design:** PII nunca enviada ao LLM (extração determinística de email/CPF/telefone); anonimização estrutural; storage cifrado com chave fora do versionamento.
- **Governança de segurança para agentes de IA:** homologação por padrão, duplo bloqueio para produção, dry-run, proibição de logar segredos.

### 4.11 Engenharia de IA / LLM (fronteira mais recente)

- **Orquestração multiagente** com mediação de conflitos por prioridade de domínio (ex.: `segurança > qualidade > produtividade > custo`), inclusive **duas implementações paralelas** (LangGraph em Python × Mastra em TypeScript) para paridade semântica e observabilidade.
- **MCP (Model Context Protocol) nos dois sentidos:** como cliente (docs, filesystem, GitHub, CodeSandbox…) e como **servidor que re-expõe os próprios agentes/workflows como tools MCP** — ponte poliglota entre orquestração TS e cálculo pesado em Python.
- **Geração de código por streaming incremental:** parser stateful de artifacts XML tolerante a **fronteira de token cortado**, escrevendo arquivos em WebContainer em tempo real; multi-provider (6 LLMs, Claude como principal) com **prompt caching** e **failover de rate limit** (429/529 → chave low-QoS).
- **Assistente de IA embutido, tenant-aware, com tool-calling e RBAC em defesa-em-profundidade:** ~24 tools consultando dados reais, gating por perfil que filtra o schema enviado ao LLM **e** revalida na execução; governança de custo (registro de modelo/tokens/duração).
- **RAG em três sabores:** com `pgvector` no banco; **sem banco vetorial** (catálogo em Parquet/polars em memória, por decisão de ADR); e ChromaDB/FAISS customizado, com **cross-encoder reranking** e avaliação IR (Top-k, MRR, MAP, nDCG).
- **ML clássico + XAI serverless:** DBSCAN/KDTree inline (Smile) em Lambda Quarkus native, com *feature importances* (estilo SHAP) embutidas no contrato de resposta para explicabilidade.
- **Padrão "determinístico-primeiro" para conter custo de LLM:** validação por dígito verificador / regras antes de acionar a visão por IA; watermark de sincronização incremental; anti-alucinação impondo que todo dado da IA apareça **literalmente** no documento.
- **Estratégia de extração plugável** (Strategy pattern: heurística → LLM → CRF) com *provenance* auditável por campo e gate de licença (só modelos Apache/MIT; rejeição consciente de libs AGPL).
- **Visão computacional:** pipeline desacoplado por estágios (segmentação → normalização → embeddings DINOv2 → scoring), com **stub determinístico** semeado por SHA-256 exercitando todo o fluxo/persistência/painel enquanto os modelos reais entram estágio a estágio. Em outro domínio, **contagem automática de objetos** (pós-larvas) por IA a partir de imagens de campo, com extração de métricas agregadas (motilidade, comprimento médio) e integração backend↔IA↔mobile.
- **GAN de super-resolução espectral** (U-Net 1D com skip connections, loss adversarial + MSE, STFT/iSTFT) exposta como microserviço serverless.

---

## 5. Domínios de negócio (anonimizados)

Amplitude de domínios em que atuei, cada um resumido pelo seu **desafio-assinatura**:

1. **Telecom / redes ópticas passivas (GPON/FTTH)** — ingestão de telemetria de equipamentos (SNMP/syslog/TR-069) e **correlação de causa-raiz** em topologia física; migração de monólito Spring Cloud on-premise para serverless Quarkus native.
2. **Varejo / PDV / ERP SaaS multi-loja** — núcleo transacional Java EE/WildFly com 200+ entidades, emissão fiscal, integrações de marketplace e WhatsApp event-driven, e portes para mobile (Kotlin Multiplatform, Flutter) e desktop (Tauri/Rust).
3. **ERP para indústria têxtil/confecção** — grade cor×tamanho, oficinas terceirizadas (façon) como cidadão de primeira classe, integração fiscal desacoplada e assistente de IA com tool-calling.
4. **Documentos fiscais eletrônicos (NF-e/NFC-e)** — microserviço com configuração fiscal 100% dinâmica por request (multi-empresa/UF sem redeploy), assinatura XML-DSig e comunicação com SEFAZ.
5. **Gestão educacional (escola de idiomas)** — plataforma acadêmica ampla (86 entidades), realtime via MQTT, auditoria a nível de Hibernate e motor de risco de evasão.
6. **Integração com sistemas governamentais de educação** — auditoria financeira "Previsto × Executado", ETL massivo de matrículas, ingestão por e-mail, dashboards e backup/réplica de PostgreSQL de produção.
7. **RH / recrutamento com IA** — parsing de currículos, ranking de candidatos, arquitetura orquestrador (Spring) × workers de IA (Python) via RabbitMQ.
8. **Indústria 4.0 / alimentos** — recomendações prescritivas por agentes de IA multiagente a partir de dados de chão de fábrica.
9. **IoT industrial (telemetria de gases)** — monitoramento de pressão em tempo real com **detecção de eventos críticos** (streaming, cooldown, histórico de alertas), provisionamento seguro de fleet, persistência híbrida SQL+NoSQL.
10. **Automação de postos de combustível** — fidelidade acionada por reconhecimento de placas (visão computacional) com autorização física de bombas; migração *strangler fig* de monólito Django para microserviços Go.
11. **Fintech de contas a pagar (PF/MEI)** — extração de boletos de e-mail com pipeline anti-alucinação (validação por dígito verificador antes da IA), invariantes de negócio impostas no banco.
12. **Self-checkout autônomo para condomínios** — totem Android em modo kiosk 24/7, backend Supabase com RLS, dois gateways de pagamento e controle de estoque atômico.
13. **Portal cartorário/jurídico** — site institucional + portal autenticado com editor de estatuto jurídico e **bridge sobre legado PHP/Vue sem API**.
14. **Guia digital comercial/turístico** — arquitetura tri-plataforma (web admin + mobile + API) com identidade Cognito compartilhada e **migração de nuvem sem downtime preservando a URL pública**.
15. **Engenharia acústica de edificações** — pipeline distribuído de IA (DXF → GAN → auralização) orquestrado em Java sobre microserviços serverless.
16. **Inspeção de qualidade têxtil por visão computacional** — comparação par-a-par few-shot com revisão humana por exceção e chain-of-custody anti-fraude.
17. **Aquicultura / carcinicultura (visão computacional)** — contagem automática de pós-larvas de camarão por IA a partir de imagens de campo, com métricas de motilidade e comprimento médio, storage de objetos e app mobile offline-first sincronizado por JWT.
18. **Apoio à decisão clínica (RAG médico)** — predição CID-10 com **roteamento LLM local × remoto** otimizando privacidade, custo e pegada de carbono.
19. **Plataformas de agentes de IA / ferramentas de desenvolvedor** — geração de apps full-stack e de specs de produto por orquestração multiagente (Mastra/Convex/MCP), incluindo **geração de backends (Spring/NestJS/FastAPI) e frontends a partir de DDL/SQL e de linguagem natural**.
20. **Automação IoT embarcada** — controle ambiental com leitura de sensor por bit-banging e filtragem de ruído.
21. **P&D em IA aplicada (propostas de concepção madura)** — otimização combinatória, sistemas multiagentes com LLMs locais para extração adaptativa, credit scoring com análise de sobrevivência, psicometria (IRT) aplicada a segurança comportamental, geração de código por agentes.

---

## 6. Diferenciais e maturidade de engenharia

- **Golden path próprio e replicável:** stack de referência consolidada (Spring Boot 3.x/Java 21 + PostgreSQL + Flyway + Cognito + Angular signals) com convenções codificadas em skills/playbooks — `BaseEntity`, paginação padrão, MapStruct, RFC 7807, optimistic locking, soft delete — permitindo reuso consistente entre projetos.
- **Convívio maduro entre gerações de arquitetura:** legado (Java EE/WildFly, Spring Cloud Netflix, Delphi, Angular 5) migrando ativamente para o estado da arte (Quarkus native, EKS/Terraform, Angular 21 zoneless, Kotlin Multiplatform).
- **Poliglota por necessidade, não por moda:** Rust onde importa binário/performance, Go em microserviços de alta cardinalidade, Python para IA/DS, TypeScript para agentes e frontend.
- **Disciplina de qualidade e governança:** ADRs formais, ArchUnit/Testcontainers, cobertura ≥80%, auditorias OWASP explícitas, benchmarks com **gates métricos e resultados medidos** (não estimados), e — traço raro — **autoconsciência dos gaps** (documentar honestamente onde doc diverge do código, ou onde métricas reais ficam abaixo do ideal).
- **Consciência de custo com IA:** arquiteturas "determinístico-primeiro", watermarks de sincronização, prompt caching e failover — mantendo metas de custo operacional explícitas.
- **Segurança em amadurecimento visível:** dos primeiros PoCs (tenant por header) aos produtos com fail-closed, AES-256-GCM, least-privilege e credenciais STS efêmeras zero-trust.

---

*Documento gerado a partir da análise técnica do acervo pessoal de projetos. Empresas, produtos e clientes intencionalmente omitidos.*
