# Modelo de Dados e Sistema de Entidades

## Arquitetura do Banco de Dados

O Timelinize utiliza **SQLite** como base de dados principal, com uma arquitetura sofisticada que suporta relacionamentos complexos entre diferentes tipos de dados.

### Estrutura Principal

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│   Items     │────│ Relationships│────│  Entities   │
│             │    │              │    │             │
│ - ID        │    │ - Relation   │    │ - ID        │
│ - Timestamp │    │ - From/To    │    │ - Name      │
│ - Content   │    │ - Metadata   │    │ - Type      │
│ - DataFile  │    │              │    │ - Picture   │
└─────────────┘    └──────────────┘    └─────────────┘
       │                   │                   │
       │                   │                   │
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│Classifications│   │  Relations   │    │ Attributes  │
│             │    │              │    │             │
│ - Standard  │    │ - Label      │    │ - Name      │
│ - Name      │    │ - Directed   │    │ - Value     │
│ - Labels    │    │ - Subordinate│    │ - Location  │
└─────────────┘    └──────────────┘    └─────────────┘
```

## Entidades - O Coração do Sistema

### Conceito de Entidade
Uma **entidade** representa qualquer pessoa, lugar, ou coisa (substantivos) no sistema:
- **Pessoas**: Amigos, família, contatos
- **Organizações**: Empresas, grupos, instituições  
- **Lugares**: Localizações específicas
- **Animais**: Pets, animais de estimação

### Sistema de Atributos
Cada entidade possui **atributos** que a descrevem:

```sql
CREATE TABLE "attributes" (
    "id" INTEGER PRIMARY KEY,
    "name" TEXT NOT NULL,      -- tipo do atributo (email, phone, etc)
    "value",                   -- valor principal
    "alt_value" TEXT,          -- valor alternativo/normalizado
    "longitude" REAL,          -- coordenada geográfica
    "latitude" REAL,           -- coordenada geográfica
    "altitude" REAL,           -- altitude
    "metadata" TEXT            -- dados extras em JSON
);
```

### Relacionamentos Entre Entidades
O sistema suporta relacionamentos direcionais complexos:

```sql
CREATE TABLE "relationships" (
    "relation_id" INTEGER,     -- tipo de relacionamento
    "from_item_id" INTEGER,    -- item de origem
    "to_item_id" INTEGER,      -- item destino
    "from_attribute_id" INTEGER, -- atributo origem
    "to_attribute_id" INTEGER,   -- atributo destino
    "start" INTEGER,           -- início da relação
    "end" INTEGER,             -- fim da relação
    "value",                   -- valor da relação
    "metadata" TEXT            -- dados extras
);
```

## Sistema de Items

### Estrutura de Item
Cada **item** representa um dado específico importado:

```sql
CREATE TABLE "items" (
    "id" INTEGER PRIMARY KEY,
    "data_source_id" INTEGER,  -- origem dos dados
    "classification_id" INTEGER, -- tipo de conteúdo
    "original_id_hash" BLOB,   -- hash para detectar duplicatas
    "timestamp" INTEGER,       -- quando aconteceu
    "timespan" INTEGER,        -- duração em ms
    "timezone" TEXT,           -- fuso horário original
    "data_text" TEXT,          -- conteúdo textual
    "data_file" TEXT,          -- caminho para arquivo
    "data_type" TEXT,          -- MIME type
    "metadata" TEXT,           -- metadados em JSON
    "latitude" REAL,           -- localização
    "longitude" REAL,
    "altitude" REAL,
    "coordinate_system" TEXT,
    "accuracy_meters" REAL
);
```

### Sistema de Classificação
Items são classificados automaticamente:

```sql  
CREATE TABLE "classifications" (
    "id" INTEGER PRIMARY KEY,
    "standard" BOOLEAN,        -- classificação padrão do sistema
    "name" TEXT NOT NULL,      -- nome único
    "labels" TEXT,             -- labels para UI
    "description" TEXT         -- descrição
);
```

Exemplos de classificações:
- `message` - Mensagens de texto
- `photo` - Fotografias
- `video` - Vídeos
- `location` - Dados de localização
- `contact` - Informações de contato
- `social_media_post` - Posts em redes sociais

## Recursos Avançados

### Busca Semântica com IA
```sql
CREATE TABLE "embeddings" (
    "item_id" INTEGER PRIMARY KEY,
    "vector" BLOB,             -- embedding vetorial
    "model" TEXT,              -- modelo usado
    "dimensions" INTEGER,      -- dimensões do vetor
    "created" INTEGER          -- timestamp
);
```

### Sistema de Jobs
Processamento assíncrono com estado persistente:

```sql
CREATE TABLE "jobs" (
    "id" INTEGER PRIMARY KEY,
    "type" TEXT NOT NULL,      -- import, thumbnails, etc
    "state" TEXT,              -- queued, started, succeeded, failed
    "configuration" TEXT,      -- config em JSON
    "progress" INTEGER,        -- progresso atual
    "total" INTEGER,           -- total a processar
    "checkpoint" TEXT          -- estado para resume
);
```

### Thumbnails Separados
Base de dados separada para performance:

```sql
-- thumbnails.db
CREATE TABLE "thumbnails" (
    "item_id" INTEGER PRIMARY KEY,
    "data" BLOB,               -- imagem comprimida
    "format" TEXT,             -- webp, jpeg, etc
    "width" INTEGER,
    "height" INTEGER,
    "filesize" INTEGER
);
```

## Inteligência do Sistema

### Detecção de Duplicatas
- **Hash de conteúdo**: Evita reimportação de dados idênticos
- **Hash de ID original**: Detecta mesmo item de sources diferentes
- **Comparação semântica**: IA identifica conteúdo similar

### Normalização Automática
- **Entidades**: Mesmo pessoa reconhecida em diferentes sources
- **Localizações**: Coordenadas normalizadas e agrupadas
- **Timestamps**: Fusos horários convertidos e normalizados
- **Metadados**: Estruturação automática de dados não-estruturados

### Sistema de Relacionamentos
- **Conversas**: Agregação de mensagens entre pessoas
- **Localização**: Items conectados por proximidade temporal/espacial
- **Conteúdo**: Fotos/vídeos relacionados por pessoas ou eventos
- **Hierarquias**: Threads, respostas, comentários

---

**Anterior**: [← Arquitetura DataSources](02-arquitetura-datasources.md) | **Próximo**: [Aspectos Valiosos →](04-aspectos-valiosos.md)

