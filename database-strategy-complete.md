# Estratégia Completa de Modelagem do Banco de Dados - Sistema de Tracking UTM

## 🏗️ Estratégia Completa de Modelagem do Banco de Dados

### 📋 Estrutura de Tabelas Proposta:

#### 1. **domains** (Domínios/Sites)
**Propósito**: Catalogar todos os domínios/sites monitorados
- `domain_id` (UUID) - PK
- `domain_name` (VARCHAR)
- `domain_url` (VARCHAR)
- `is_active` (BOOLEAN)
- `created_at` (TIMESTAMP)
- `metadata` (JSONB) - configurações específicas

#### 2. **landing_pages** (Páginas de Destino)
**Propósito**: Registrar cada LP específica
- `page_id` (UUID) - PK
- `domain_id` (UUID) - FK
- `page_url` (TEXT)
- `page_name` (VARCHAR)
- `page_variant` (VARCHAR) - para A/B testing
- `is_control` (BOOLEAN) - variante controle
- `ab_test_id` (UUID) - agrupa variantes
- `is_active` (BOOLEAN)
- `created_at` (TIMESTAMP)

#### 3. **sessions** (Sessões de Usuário)
**Propósito**: Agregar toda atividade de um usuário
- `session_id` (VARCHAR) - PK (o mesmo gerado pelo frontend)
- `domain_id` (UUID) - FK
- `first_page_id` (UUID) - FK
- `visitor_id` (VARCHAR) - hash do IP+UserAgent
- `started_at` (TIMESTAMP)
- `last_activity_at` (TIMESTAMP)
- `total_page_views` (INTEGER)
- `total_events` (INTEGER)
- `converted` (BOOLEAN)
- `metadata` (JSONB)

#### 4. **utm_tracking** (Rastreamento UTM)
**Propósito**: Primeira entrada com parâmetros UTM
- `tracking_id` (UUID) - PK
- `session_id` (VARCHAR) - FK
- `page_id` (UUID) - FK
- `utm_source` (VARCHAR)
- `utm_medium` (VARCHAR)
- `utm_campaign` (VARCHAR)
- `utm_id` (VARCHAR)
- `utm_term` (VARCHAR)
- `utm_content` (VARCHAR)
- `gclid` (VARCHAR) - Google Click ID
- `fbclid` (VARCHAR) - Facebook Click ID
- `ttclid` (VARCHAR) - TikTok Click ID
- `li_fat_id` (VARCHAR) - LinkedIn First-Party Ad Tracking ID
- `device` (VARCHAR)
- `match_type` (VARCHAR)
- `attribution_model` (VARCHAR)
- `referrer` (TEXT)
- `page_url` (TEXT)
- `url_params` (JSONB) - outros parâmetros
- `user_agent` (TEXT)
- `client_ip` (INET)
- `created_at` (TIMESTAMP)

#### 5. **page_views** (Visualizações de Página)
**Propósito**: Cada página vista na sessão
- `view_id` (UUID) - PK
- `session_id` (VARCHAR) - FK
- `page_id` (UUID) - FK
- `view_order` (INTEGER) - ordem na sessão
- `time_on_page` (INTEGER) - segundos
- `exit_page` (BOOLEAN)
- `created_at` (TIMESTAMP)

#### 6. **events** (Eventos de Interação)
**Propósito**: Ações específicas (cliques, scroll, etc)
- `event_id` (UUID) - PK
- `session_id` (VARCHAR) - FK
- `page_id` (UUID) - FK
- `event_type` (VARCHAR) - click, scroll, video_play, etc
- `event_category` (VARCHAR)
- `event_action` (VARCHAR)
- `event_label` (VARCHAR)
- `event_value` (NUMERIC)
- `element_id` (VARCHAR)
- `element_class` (VARCHAR)
- `event_data` (JSONB)
- `created_at` (TIMESTAMP)

#### 7. **leads** (Leads Convertidos)
**Propósito**: Formulários preenchidos
- `lead_id` (UUID) - PK
- `session_id` (VARCHAR) - FK
- `utm_tracking_id` (UUID) - FK
- `page_id` (UUID) - FK
- `external_id` (VARCHAR) - ID do CRM
- `name` (VARCHAR)
- `email` (VARCHAR)
- `hashed_email` (VARCHAR) - SHA256 do email
- `company` (VARCHAR)
- `phone` (VARCHAR)
- `hashed_phone` (VARCHAR) - SHA256 do telefone
- `extensions` (INTEGER)
- `message` (TEXT)
- `form_location` (VARCHAR)
- `lead_score` (VARCHAR)
- `is_business_email` (BOOLEAN)
- `email_domain` (VARCHAR)
- `time_to_conversion` (INTEGER)
- `created_at` (TIMESTAMP)
- `metadata` (JSONB)

#### 8. **lead_stages** (Estágios do Lead)
**Propósito**: Acompanhar evolução do lead no funil de vendas
- `stage_id` (UUID) - PK
- `lead_id` (UUID) - FK
- `stage_name` (VARCHAR) - 'new', 'qualified', 'proposal', 'negotiation', 'won', 'lost'
- `stage_value` (DECIMAL) - valor do negócio
- `probability` (INTEGER) - % de fechar
- `changed_by` (VARCHAR) - quem mudou
- `reason` (TEXT) - motivo da mudança
- `created_at` (TIMESTAMP)

#### 9. **offline_conversions** (Conversões Offline)
**Propósito**: Registrar vendas fechadas para enviar às plataformas
- `conversion_id` (UUID) - PK
- `lead_id` (UUID) - FK
- `original_tracking_id` (UUID) - FK para utm_tracking
- `gclid` (VARCHAR) - Google Click ID original
- `fbclid` (VARCHAR) - Facebook Click ID
- `ttclid` (VARCHAR) - TikTok Click ID
- `li_fat_id` (VARCHAR) - LinkedIn ID
- `conversion_value` (DECIMAL) - valor da venda
- `conversion_currency` (VARCHAR) - BRL, USD
- `conversion_date` (DATE) - quando fechou
- `conversion_name` (VARCHAR) - 'purchase', 'contract_signed'
- `sync_status` (JSONB) - {google: 'sent', meta: 'pending', tiktok: 'failed'}
- `created_at` (TIMESTAMP)
- **Índice único**: (lead_id, conversion_name)

#### 10. **platform_sync_log** (Log de Sincronização com Plataformas)
**Propósito**: Rastrear envios de conversões para cada plataforma
- `sync_id` (UUID) - PK
- `conversion_id` (UUID) - FK
- `platform` (VARCHAR) - 'google_ads', 'meta', 'linkedin', 'tiktok'
- `sync_status` (VARCHAR) - 'success', 'failed', 'pending'
- `request_payload` (JSONB)
- `response_data` (JSONB)
- `error_message` (TEXT)
- `http_status_code` (INTEGER)
- `synced_at` (TIMESTAMP)

#### 11. **integrations_log** (Log de Integrações CRM/Webhooks)
**Propósito**: Rastrear envios para CRM/webhooks
- `log_id` (UUID) - PK
- `lead_id` (UUID) - FK
- `integration_type` (VARCHAR) - crm, webhook, email
- `integration_name` (VARCHAR) - hubspot, salesforce, pipedrive, etc
- `status` (VARCHAR) - success, failed, pending
- `request_data` (JSONB)
- `response_data` (JSONB)
- `error_message` (TEXT)
- `retry_count` (INTEGER)
- `created_at` (TIMESTAMP)
- `processed_at` (TIMESTAMP)

#### 12. **visitor_journeys** (Jornadas de Visitantes)
**Propósito**: Agregar todas as sessões de um visitante
- `journey_id` (UUID) - PK
- `visitor_id` (VARCHAR)
- `domain_id` (UUID)
- `first_visit_at` (TIMESTAMP)
- `last_visit_at` (TIMESTAMP)
- `total_sessions` (INTEGER)
- `total_page_views` (INTEGER)
- `converted` (BOOLEAN)
- `conversion_session_id` (VARCHAR)

### 🔄 Estratégia para Retorno de Usuários:

**Identificação de Visitantes Recorrentes:**
1. Criar `visitor_id` = hash(client_ip + user_agent + domain_id)
2. Se usuário retorna com novo session_id mas mesmo visitor_id:
   - Incrementar campo `return_visits` na tabela sessions
   - Criar nova entrada em utm_tracking se houver novos UTMs
   - Manter histórico completo de todas as visitas

### 🎯 Índices Estratégicos:

**Para Performance em Queries Analíticas:**
- `utm_tracking`: índices em (created_at, utm_source, utm_campaign, session_id, gclid, fbclid)
- `leads`: índices em (created_at, email, lead_score, session_id, external_id)
- `sessions`: índices em (started_at, converted, domain_id, visitor_id)
- `events`: índices em (created_at, event_type, session_id)
- `offline_conversions`: índices em (lead_id, conversion_date, sync_status)
- `lead_stages`: índices em (lead_id, created_at, stage_name)

### 📊 Views Materializadas para Dashboard Real-time:

1. **daily_conversions_summary**
   - Agregações por dia/hora, campanha, fonte
   - Refresh a cada 5 minutos

2. **campaign_performance**
   - Métricas de conversão por campanha
   - Lead score médio, tempo de conversão
   - ROI incluindo conversões offline

3. **visitor_behavior_summary**
   - Páginas por sessão, eventos por tipo
   - Padrões de navegação

4. **offline_conversion_metrics**
   - Taxa de conversão lead → venda por fonte
   - Tempo médio do ciclo de venda
   - Valor médio por canal

### 🗓️ Estratégia de Particionamento:

**Por mês em tabelas grandes:**
- utm_tracking_2025_01, utm_tracking_2025_02...
- events_2025_01, events_2025_02...
- leads_2025_01, leads_2025_02...
- Facilita arquivamento após 6 meses
- Melhora performance de queries

### 🔒 Considerações de Privacidade:

1. **Anonimização após 6 meses:**
   - Remover client_ip
   - Manter apenas hashed_email e hashed_phone
   - Preservar dados agregados

2. **Campos Sensíveis:**
   - Criptografar phone/email em repouso
   - Logs de acesso às tabelas de leads
   - Hashear dados antes de enviar para plataformas

### 🔄 Fluxo de Conversão Offline:

```
1. Lead capturado → Registro em 'leads'
2. CRM atualiza → Novo registro em 'lead_stages'
3. Venda fechada → stage_name = 'won'
4. Trigger cria → Registro em 'offline_conversions'
5. Job processa → Envia para APIs (Google, Meta, etc)
6. Registra resultado → 'platform_sync_log'
```

### 📅 Janelas de Atribuição por Plataforma:

- **Google Ads**: até 90 dias após o clique
- **Meta (Facebook/Instagram)**: até 28 dias
- **LinkedIn**: até 90 dias
- **TikTok**: até 28 dias

### 💡 Benefícios da Estrutura:

1. **Rastreamento Completo**: Do primeiro clique até a venda offline
2. **Otimização de Campanhas**: Plataformas aprendem quais cliques viram vendas
3. **ROAS Real**: Medir retorno verdadeiro incluindo vendas offline
4. **Smart Bidding**: Google/Meta ajustam lances para trazer leads que fecham
5. **Audiências Similar**: Criar lookalikes de quem realmente compra
6. **Multi-domínio**: Suporta múltiplos sites com análises consolidadas
7. **Escalabilidade**: Particionamento e índices otimizados para performance

Esta estrutura completa permite rastreamento end-to-end, desde o primeiro clique até a venda offline, com capacidade de otimizar campanhas baseado em vendas reais!