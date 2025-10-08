# Códigos de Migração e Mapping - O Tesouro Escondido

## 🏆 Por Que Este Código É Extremamente Valioso

O Timelinize contém **implementações completas de parsers e importadores** para 22+ plataformas diferentes. Este é um **repositório de conhecimento técnico** que representa:

- 🔓 **Engenharia reversa** de formatos proprietários
- 🗺️ **Mapping** de estruturas de dados complexas
- 🔍 **Reconhecimento automático** de formatos
- ⚡ **Otimizações** específicas por plataforma
- 🛡️ **Tratamento robusto** de edge cases

**Este código economizaria meses de trabalho** para qualquer um que precise integrar com essas plataformas.

---

## 📊 Inventário dos Importadores

### Redes Sociais & Comunicação

#### **Facebook** (`datasources/facebook/`)
```go
// Arquivos: archive.go, messages.go, models.go, facebook.go
// ~2000+ linhas de código

Capacidades:
✓ Posts e timeline completa
✓ Mensagens e conversas
✓ Fotos e vídeos com metadados
✓ Comentários e reações
✓ Amigos e relacionamentos
✓ Múltiplos formatos de archive (HTML, JSON)
✓ Suporte a archives multi-parte
```

**Técnicas valiosas**:
- Parsing de HTML estruturado do Facebook
- Extração de timestamps em múltiplos formatos
- Reconstrução de threads de conversas
- Detecção de formato por era (Facebook mudou estruturas várias vezes)

#### **Instagram** (`datasources/instagram/`)
```go
// Arquivos: instagram.go, models.go
// ~250+ linhas

Capacidades:
✓ Posts e stories
✓ DMs (Direct Messages)
✓ Likes e comentários
✓ Seguidores e seguindo
✓ Profile information completo
```

**Código de exemplo - Extração de perfil**:
```go
owner := timeline.Entity{
    Name: personalInfo.Name.Value,
    Attributes: []timeline.Attribute{
        {
            Name:     "instagram_username",
            Value:    personalInfo.Username.Value,
            Identity: true,
        },
        {
            Name:        timeline.AttributePhoneNumber,
            Value:       personalInfo.PhoneNumber.Value,
            Identifying: true,
        },
        {
            Name:        timeline.AttributeEmail,
            Value:       personalInfo.Email.Value,
            Identifying: true,
        },
    },
}
```

#### **Twitter/X** (`datasources/twitter/`)
```go
// Arquivos: twitter.go, archives.go, models.go, api.go.v1, api.go.v2
// ~500+ linhas

Capacidades:
✓ Tweets e retweets
✓ Threads completos
✓ Mídia (fotos/vídeos)
✓ Likes e replies
✓ Followers/following
✓ Suporte a API v1 e v2
✓ Archive exports
```

**Inovação técnica**:
- Múltiplas versões da API suportadas
- Fallback entre API e archive
- Reconstrução de threads distribuídos

#### **WhatsApp** (`datasources/whatsapp/`)
```go
// Arquivos: whatsapp.go, attachments.go, locations.go, polls.go, splitter.go
// ~400+ linhas

Capacidades:
✓ Mensagens de texto
✓ Mídia (fotos, vídeos, áudio, documentos)
✓ Localizações compartilhadas
✓ Polls (enquetes)
✓ Múltiplos formatos de export
✓ Detecção de idioma/formato regional
```

**Algoritmo sofisticado**:
```go
// splitter.go - Parser customizado para formato de chat
func chatSplit(data []byte, atEOF bool) (advance int, token []byte, err error) {
    // Lógica complexa para detectar início de mensagens
    // Suporta diferentes formatos de timestamp regionais
    // Trata continuações de mensagens multi-linha
}
```

#### **Telegram** (`datasources/telegram/`)
- Parsing de JSON export
- Stickers e mídia
- Canais e grupos

---

### Dados de Localização

#### **Google Location History** (`datasources/googlelocation/`)
```go
// Arquivos: googlelocation.go, locproc.go, simplify.go, semanticlocations.go
// ~1500+ linhas - MUITO SOFISTICADO

Capacidades:
✓ Processamento de arquivos JSON gigantes (GBs)
✓ Simplificação de trajectórias GPS
✓ Clustering de pontos próximos
✓ Reconhecimento de localizações semânticas
✓ Streaming eficiente
✓ Múltiplos formatos de Takeout
```

**Algoritmos valiosos**:

1. **Simplificação de Paths** (Douglas-Peucker adaptado):
```go
type LocationProcessingOptions struct {
    // 1-10: agressividade da simplificação
    // Remove pontos desnecessários mantendo shape geral
    Simplification float64 `json:"simplification,omitempty"`
    
    // Clustering de pontos próximos
    ClusteringCoefficient float64 `json:"clustering_coefficient,omitempty"`
}
```

2. **Streaming de Dados Massivos**:
```go
// Processa arquivos de GBs sem carregar tudo na memória
type LocationSource interface {
    NextLocation(ctx context.Context) (*Location, error)
}
```

3. **Semantic Locations** - Identifica lugares importantes:
- Casa, trabalho, locais frequentes
- Análise temporal de permanência
- Clustering geoespacial inteligente

#### **GPX/KML/GeoJSON** (`datasources/gpx/`, `kmlgx/`, `geojson/`)
- Parsers completos para formatos geo-padrão
- Suporte a extensões (KML with Google extensions)
- Conversão unificada para modelo interno

---

### Dados de Mídia e Arquivos

#### **Media Files** (`datasources/media/`)
```go
// Arquivos: media.go, metadata.go, motionpictures.go
// ~800+ linhas - EXTRAÇÃO COMPLETA DE METADADOS

Capacidades:
✓ EXIF completo (fotos)
✓ XMP (Adobe metadata)
✓ IPTC (press photos)
✓ MP4/QuickTime metadata
✓ Live Photos da Apple
✓ Música (ID3 tags)
✓ Localização de EXIF
✓ Timestamps de múltiplas fontes
```

**Técnica avançada - Extração multi-fonte**:
```go
func ExtractAllMetadata(logger *zap.Logger, fsys fs.FS, path string, 
                        item *timeline.Item, policy timeline.MetadataMergePolicy) {
    // 1. Tenta EXIF
    exifData := extractEXIF()
    
    // 2. Tenta XMP (Adobe)
    xmpData := extractXMP()
    
    // 3. Tenta ID3 (música)
    id3Data := extractID3()
    
    // 4. Tenta MP4 metadata
    mp4Data := extractMP4Metadata()
    
    // 5. Merge inteligente baseado em política
    mergeMetadataByPolicy(policy, exifData, xmpData, id3Data, mp4Data)
}
```

**Extração de coordenadas**:
```go
// Suporta múltiplos sistemas de coordenadas
// Converte entre formatos (DMS, decimal, etc)
// Extrai altitude, accuracy, speed
lat, lon, alt := extractGPSFromEXIF(exifData)
```

---

### Backups de Dispositivos

#### **iPhone Backup** (`datasources/iphone/`)
```go
// Arquivos: iphone.go, messages.go, photos.go, addressbook.go, cameraroll.go
// ~600+ linhas

Capacidades:
✓ SMS/iMessage
✓ Camera Roll
✓ Address Book (contatos)
✓ Call history
✓ Estrutura completa de backup iOS
```

**Conhecimento valioso**:
- Estrutura interna de backups iTunes/Finder
- Schema de banco SQLite do iOS
- Localização de arquivos por hash
- Parsing de formatos proprietários Apple

#### **Apple Photos** (`datasources/applephotos/`)
```go
// ~200+ linhas

Capacidades:
✓ Library completa
✓ Albums e organizações
✓ Faces detectados
✓ Memories
✓ Shared albums
```

**Acesso a estruturas internas**:
- `Photos.sqlite` schema
- Parsing de metadados proprietários
- Linking com arquivos de mídia

---

### Dados de Email e Contatos

#### **Email** (`datasources/email/`)
```go
// Parsing genérico de emails
Formatos suportados:
✓ MBOX
✓ EML
✓ MSG (Outlook)
✓ PST (via conversão)
```

#### **Gmail** (`datasources/gmail/`)
- Integração com API do Gmail
- OAuth2 completo
- Parsing de estruturas especiais

#### **vCard & Contact Lists** (`datasources/vcard/`, `contactlist/`)
```go
// Arquivos: vcard.go, contactlist.go, formats.go, recognize.go

Capacidades:
✓ vCard 2.1, 3.0, 4.0
✓ CSV genérico
✓ Excel
✓ Google Contacts
✓ Apple Contacts
✓ Reconhecimento automático de formato
```

**Reconhecimento inteligente de colunas**:
```go
// Detecta automaticamente colunas de:
// - Nome, sobrenome, nome completo
// - Email, telefone
// - Endereço, cidade, estado
// - Datas de nascimento
// - Empresas, cargos
```

---

### Serviços Especializados

#### **GitHub** (`datasources/github/`)
```go
// Arquivos: github.go, github_test.go
// ~150+ linhas

Capacidades:
✓ Starred repositories
✓ Activity timeline
✓ Contributions
✓ Issues e PRs
```

#### **Strava** (`datasources/strava/`)
```go
// Atividades físicas
Capacidades:
✓ Workouts e atividades
✓ GPS tracks
✓ Estatísticas e métricas
```

#### **Flighty** (`datasources/flighty/`)
```go
// Dados de voos
Arquivos: flighty.go, csvfields.go, types.go

Capacidades:
✓ Histórico completo de voos
✓ Aeroportos (incluindo DB interno!)
✓ Delays e cancelamentos
✓ Estatísticas de viagem
```

**Banco de dados incluído**:
```go
// internal/airports/airports.csv
// ~10,000 aeroportos mundiais
// IATA codes, coordenadas, timezones
```

---

## 🛠️ Técnicas Compartilhadas e Padrões

### 1. **Reconhecimento Automático de Formato**

Todos os datasources implementam:
```go
func (i *Importer) Recognize(ctx context.Context, 
                             dirEntry timeline.DirEntry, 
                             params timeline.RecognizeParams) (timeline.Recognition, error) {
    // Retorna confidence score 0.0 - 1.0
    // Sistema escolhe automaticamente o importador correto
}
```

**Técnicas de detecção**:
- Arquivos marcadores específicos
- Estrutura de diretórios
- Magic bytes em arquivos
- Padrões de naming
- Conteúdo JSON/XML característico

### 2. **Streaming Eficiente**

Para arquivos grandes:
```go
// NÃO carrega tudo na memória
scanner := bufio.NewScanner(file)
for scanner.Scan() {
    processLine(scanner.Text())
}
```

### 3. **Normalização de Timestamps**

Cada datasource lida com seus formatos peculiares:
```go
// Facebook: timestamps Unix em segundos
// Instagram: ISO 8601
// WhatsApp: formatos regionais variados
// Google: milissegundos Unix

// Todos normalizados para time.Time UTC
```

### 4. **Detecção de Duplicatas**

```go
// Hash de conteúdo original
originalIDHash := computeHash(originalID, dataSourceID)

// Hash de conteúdo
contentHash := blake3.Sum256(content)

// Sistema detecta automaticamente duplicatas
```

### 5. **Extração de Entidades**

Código compartilhado para identificar:
- Pessoas (nomes, usernames, IDs)
- Localizações (endereços, coordenadas)
- Organizações
- Relacionamentos entre entidades

---

## 💎 Os Mais Valiosos Para Referência

### Top 5 Implementações Mais Sofisticadas:

1. **Google Location History** (`googlelocation/`)
   - Algoritmos de simplificação de paths
   - Clustering geoespacial
   - Streaming de arquivos gigantes
   - ~1500 linhas de código avançado

2. **Media Metadata** (`media/`)
   - Extração multi-formato (EXIF, XMP, IPTC, MP4)
   - Merge inteligente de metadados
   - Suporte a Live Photos
   - ~800 linhas

3. **WhatsApp** (`whatsapp/`)
   - Parser customizado de formato texto
   - Suporte multi-regional
   - Tratamento de attachments complexo
   - ~400 linhas

4. **Facebook Archive** (`facebook/`)
   - Múltiplos formatos de export
   - Parsing de HTML estruturado
   - Reconstrução de threads
   - ~2000+ linhas

5. **iPhone Backup** (`iphone/`)
   - Acesso a estruturas internas iOS
   - SQLite schema reverso
   - ~600 linhas

---

## 🎯 Valor Prático

### Para Desenvolvedores:

**Se você precisa integrar com qualquer dessas plataformas**, este código:
- ✅ Economiza **meses de engenharia reversa**
- ✅ Fornece **estruturas de dados documentadas**
- ✅ Inclui **tratamento de edge cases** já testado
- ✅ Mostra **boas práticas** de parsing
- ✅ Oferece **código de produção** pronto

### Para Pesquisadores:

- 📊 **Dataset real** de estruturas de dados de plataformas
- 🔬 **Referência** para estudos de privacidade
- 📖 **Documentação implícita** de formatos proprietários
- 🧪 **Base** para ferramentas de análise

### Para Empresas:

- 💼 **Integração rápida** com múltiplas plataformas
- 🔄 **Migração de dados** entre sistemas
- 📦 **Export/Import** de dados de clientes
- 🛡️ **Compliance** com GDPR (portabilidade de dados)

---

## 📚 Como Usar Como Referência

### Exemplo 1: Implementar Importador de Nova Plataforma

```go
// 1. Estudar datasource similar
// Por exemplo, para adicionar TikTok, estudar Instagram

// 2. Copiar estrutura básica
type TikTokImporter struct{}

func (i *TikTokImporter) Recognize(ctx context.Context, ...) {
    // Adaptar lógica de reconhecimento
}

func (i *TikTokImporter) FileImport(ctx context.Context, ...) {
    // Adaptar lógica de processamento
}

// 3. Reusar código de utilidades
// - Parsing de JSON
// - Extração de mídia
// - Normalização de timestamps
```

### Exemplo 2: Extrair Metadados de Fotos

```go
// Código completo já pronto em media/metadata.go
picture, err := ExtractAllMetadata(logger, fs, filepath, item, policy)

// Obtém:
// - Timestamp de captura
// - Localização GPS
// - Câmera e configurações
// - Faces detectados
// - Todos os metadados EXIF/XMP
```

### Exemplo 3: Processar Location History

```go
// Algoritmos prontos em googlelocation/
processor := NewLocationProcessor(source, LocationProcessingOptions{
    Simplification: 5.0,           // Simplifica trajetos
    ClusteringCoefficient: 1.0,    // Agrupa pontos próximos
})

// Reduz milhões de pontos para quantidade gerenciável
// Mantendo fidelidade da trajetória
```

---

## 🔮 Potencial Futuro

### Novos DataSources Fáceis de Adicionar:

Com esta base de código:
- **LinkedIn** - Similar ao Facebook
- **TikTok** - Similar ao Instagram  
- **Discord** - Similar ao Telegram
- **Slack** - Similar ao WhatsApp
- **Spotify** - Similar ao Strava
- **Fitbit/Apple Health** - Padrões estabelecidos

### Bibliotecas Spin-off Possíveis:

Este código poderia gerar:
- `go-whatsapp-parser` - Parser standalone
- `exif-extractor-go` - Biblioteca de metadados
- `location-simplifier` - Algoritmos geo
- `social-media-parsers` - Collection de parsers

---

## ✨ Conclusão

O código de migração/mapping do Timelinize é um **tesouro escondido** que representa:

- 📖 **Anos de engenharia reversa** documentada
- 🔧 **Código de produção** testado e robusto
- 🎓 **Conhecimento técnico** de formatos proprietários
- ⚡ **Otimizações** específicas por plataforma
- 🛡️ **Tratamento de edge cases** real-world

**Este é possivelmente o código mais valioso do projeto** para reuso, aprendizado e referência técnica.

---

**Anterior**: [← Aspectos Valiosos](04-aspectos-valiosos.md) | **Início**: [← README](README.md)
