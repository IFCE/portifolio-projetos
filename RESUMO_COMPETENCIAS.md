# Resumo de Competências Técnicas

Engenheiro de software **full-stack e cloud**, com forte atuação em **backend distribuído, arquiteturas event-driven/serverless, multi-tenant e, mais recentemente, plataformas de IA/LLM**. Experiência comprovada de ponta a ponta — do domínio e da API à mensageria, banco, frontend, mobile e infraestrutura como código — em projetos de produção, sistemas críticos e P&D.

## Stacks que domino

- **Backend:** Java 8→21 (Spring Boot, Spring Cloud, Quarkus native/GraalVM, Jakarta EE/WildFly), Go, Node/TypeScript (Fastify, Next.js, Deno), Python (FastAPI, Django), Rust.
- **Frontend & Mobile:** Angular (5→21, signals/standalone), React 18/19, Next.js; Flutter, React Native/Expo, Kotlin Multiplatform, Android nativo.
- **Dados & Mensageria:** PostgreSQL (multi-tenant, pgvector, Flyway), MariaDB, SQL Server, DynamoDB, MongoDB, Redis; Kafka (mTLS, Debezium CDC), RabbitMQ, AWS SQS/SNS, WebSocket/SSE.
- **Cloud & Infra:** AWS (Lambda, API Gateway, Cognito, DynamoDB, S3, SQS/SNS, IoT Core, ECS/Fargate, STS, Aurora); object storage (S3, Cloudflare R2, Tigris, MinIO); Docker, Kubernetes, Linkerd; Terraform, AWS CDK, Serverless Framework; Prometheus/Grafana/OpenTelemetry.
- **IA/LLM & ML:** Agentes (Mastra, LangChain/LangGraph, MCP, tool-calling, RAG, LLM-as-judge); Anthropic Claude, OpenAI, modelos locais (Ollama); ML clássico + XAI, **visão computacional (contagem/inspeção)**, embeddings/pgvector; geração de código por agentes (Spring/NestJS/FastAPI + frontends a partir de DDL/SQL e linguagem natural).
- **Auth & Segurança:** OAuth2/OIDC (Cognito, Keycloak), JWT/RBAC, mTLS, criptografia em repouso, LGPD by design.

## Desafios que já resolvi

- **Multi-tenancy** implementado em 5 estratégias distintas (schema, filtro, database, fila por tenant, stack por cliente) — incluindo resolução de incidente de vazamento de tenant entre threads em produção.
- **Arquiteturas event-driven serverless** na AWS: correlação de causa-raiz, entrega confiável (idempotência, DLQ, retry/backoff), consumo pull-based para contornar firewall e **zero-trust com credenciais STS efêmeras**.
- **Migração de monólitos para serverless** com Quarkus native (otimização de cold start em GraalVM).
- **Integração com sistemas legados** sem API: bridge/BFF sobre legado PHP/Vue, interop criptográfico com Delphi (reimplementado em Rust), sincronização cloud↔on-premise com diff SHA-256.
- **Ingestão de dados em alto volume e fontes instáveis**: ETL massivo multithread, web scraping de portais sem API, ingestão por e-mail (IMAP), streaming de arquivos gigantes.
- **IoT edge-to-cloud**: telemetria ESP32 → MQTT/TLS → AWS IoT Core, provisionamento de fleet, persistência híbrida SQL+NoSQL, detecção de eventos críticos em tempo real (streaming + alertas).
- **Consistência distribuída com hardware físico** (WAL, reaper de estado órfão, preempção) e **concorrência financeira correta** (locking ordenado, invariantes impostas no banco).
- **Engenharia de IA/LLM**: orquestração multiagente, RAG (pgvector/vetorial/em memória), tool-calling tenant-aware com RBAC, pipelines anti-alucinação, estratégia "determinístico-primeiro" para conter custo e **geração de código por agentes** (backend/frontend a partir de DDL/SQL e linguagem natural).
- **Visão computacional**: contagem automática de objetos e inspeção de qualidade par-a-par (few-shot, DINOv2), com métricas agregadas e revisão humana por exceção.
- **Algoritmos aplicados**: grafos (Dijkstra, blast-radius), estatística implementada do zero (regressão, Kendall's Tau, bootstrap), GAN de super-resolução.

## Domínios de negócio (amostra)

Telecom/redes ópticas · Varejo/PDV/ERP · Fiscal (NF-e) · Fintech · IoT industrial · Automação com visão computacional · Aquicultura/carcinicultura · Educação · RH/recrutamento · Saúde (RAG médico) · Indústria 4.0 · Jurídico/cartorário · Plataformas de agentes de IA.

## Diferenciais

Golden path próprio replicável · Convívio maduro entre stacks legadas e estado da arte · Disciplina de qualidade (ADRs, ArchUnit, Testcontainers, cobertura ≥80%, auditorias OWASP) · Consciência de custo com IA · Poliglota por necessidade.
