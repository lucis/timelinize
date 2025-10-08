# Como Manter o Fork Sincronizado

Este repositório é um fork de [timelinize/timelinize](https://github.com/timelinize/timelinize) com documentação adicional na pasta `analise/`.

## Configuração Atual

```bash
origin      git@github.com:lucis/timelinize.git          # Seu fork
upstream    git@github.com:timelinize/timelinize.git     # Repositório original
```

## Sincronizar com o Repositório Original

### 1. Buscar atualizações do upstream

```bash
git fetch upstream
```

### 2. Mesclar mudanças do upstream na sua branch main

```bash
git checkout main
git merge upstream/main
```

### 3. Enviar as atualizações para seu fork

```bash
git push origin main
```

## Comando Único (mais rápido)

```bash
git fetch upstream && git checkout main && git merge upstream/main && git push origin main
```

## Em Caso de Conflitos

Se houver conflitos durante o merge:

1. Resolver os conflitos manualmente nos arquivos indicados
2. Adicionar os arquivos resolvidos: `git add <arquivo>`
3. Completar o merge: `git commit`
4. Fazer push: `git push origin main`

## Manter Suas Modificações

A pasta `analise/` é exclusiva deste fork e não deve conflitar com atualizações do upstream.

Para trabalhar em novas funcionalidades sem afetar a sincronização:

```bash
# Criar uma branch para suas modificações
git checkout -b minha-feature

# Fazer commits normalmente
git add .
git commit -m "descrição"

# Push para seu fork
git push origin minha-feature
```

## Verificar Status do Fork

```bash
# Ver quantos commits à frente/atrás do upstream você está
git fetch upstream
git status
git log --oneline --graph --all --decorate
```

