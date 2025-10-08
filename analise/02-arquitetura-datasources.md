# Arquitetura de DataSources

## Conceito Central

O **sistema de DataSources** é o coração do Timelinize. Cada fonte de dados implementa uma interface comum que permite:
- **Reconhecimento automático** de formatos
- **Importação padronizada** de dados
- **Processamento específico** para cada plataforma
- **Extensibilidade** para novas fontes

## DataSources Implementados

### 📱 **Dados Móveis**
- **iPhone Backup**: Contacts, Messages, Photos, Address Book
- **Apple Photos**: Library completa com metadados
- **Apple Contacts**: Agenda de contatos macOS
- **iCloud**: Sincronização de dados Apple

### 💬 **Mensagens e Comunicação**
- **WhatsApp**: Exports individuais de conversas
- **iMessage**: Mensagens do macOS/iOS
- **Telegram**: Histórico completo de mensagens
- **SMS Backup & Restore**: Backups Android
- **Email**: Importação genérica de emails

### 🌐 **Redes Sociais**
- **Facebook**: Archive completo (posts, fotos, mensagens)
- **Instagram**: Dados do takeout
- **Twitter/X**: Archives de dados pessoais
- **Google Photos**: Takeout com metadados
- **Google Voice**: Histórico de chamadas

### 📍 **Localização e Mapas**
- **Google Location History**: Timeline completo do Maps
- **GPX Files**: Tracks de GPS
- **NMEA**: Dados brutos de GPS
- **KML/KMZ**: Arquivos do Google Earth
- **GeoJSON**: Dados geoespaciais genéricos

### 🗂️ **Documentos e Arquivos**
- **Generic Files**: Importador universal de arquivos
- **Media Files**: Fotos, vídeos com metadados EXIF
- **Calendar**: Arquivos ICS/vCalendar
- **vCard**: Contatos em formato padrão

### 🔧 **Ferramentas e Serviços**
- **GitHub**: Starred repositories e atividade
- **Strava**: Atividades físicas e exercícios
- **Flighty**: Dados de voos e viagens
- **Firefox**: Histórico de navegação

### 📋 **Listas de Contatos**
- **Contact Lists**: Importador genérico para diferentes formatos
- Suporte a CSV, Excel, JSON
- Reconhecimento inteligente de campos

## Arquitetura Técnica

### Interface DataSource

```go
type DataSource struct {
    Name            string
    Title           string  
    Icon            string
    Description     string
    NewOptions      func() any
    NewFileImporter func() FileImporter
}
```

### Fluxo de Importação

1. **Reconhecimento** (`Recognize()`)
   - Análise de estrutura de arquivos/pastas
   - Confidence score (0.0 - 1.0)
   - Identificação automática de formato

2. **Importação** (`FileImport()`)
   - Processamento específico por datasource
   - Criação de Items estruturados
   - Extração de metadados e relacionamentos

3. **Processamento** (`Pipeline`)
   - Normalização de dados
   - Detecção de entidades
   - Criação de relacionamentos
   - Armazenamento em BD

### Exemplo: WhatsApp DataSource

```go
func (Importer) Recognize(ctx context.Context, dirEntry DirEntry, params RecognizeParams) (Recognition, error) {
    if dirEntry.FileExists(chatPath) {
        return Recognition{Confidence: 0.8}, nil
    }
    return Recognition{}, nil
}
```

## Pontos Fortes da Arquitetura

### 🔌 **Extensibilidade**
- Adicionar novos datasources é trivial
- Interface bem definida e documentada
- Sistema de plugins automático via imports
- Reconhecimento automático sem configuração

### 🧠 **Inteligência**
- Reconhecimento automático de formatos
- Processamento específico por plataforma
- Extração inteligente de metadados
- Detecção de duplicatas

### 📊 **Robustez**
- Processamento tolerante a falhas
- Logs detalhados de importação
- Rollback em caso de erro
- Validação de dados

### ⚡ **Performance**
- Processamento em streaming
- Paralelização automática
- Cache inteligente
- Otimizações específicas por formato

## Capacidades Especiais

### **Google Location History**
- Processamento de arquivos JSON gigantes (GB)
- Simplificação de trajectórias GPS
- Reconhecimento de localizações semânticas
- Compressão inteligente de pontos

### **Facebook Archive**
- Parsing completo de estrutura HTML
- Extração de metadados de fotos
- Reconstrução de threads de conversas
- Processamento de diferentes formatos por era

### **Media Files**
- Extração de EXIF/XMP/IPTC
- Processamento de Live Photos
- Detecção de duplicatas por hash
- Geração de thumbnails automática

---

**Anterior**: [← Visão Geral](01-visao-geral.md) | **Próximo**: [Modelo de Dados →](03-modelo-dados.md)

