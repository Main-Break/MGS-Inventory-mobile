# mgs-inventario-mobile

App Android (Kotlin) do projeto de **inventário de estoque por foto** da MGS Plásticos de Engenharia.
O operador fotografa a pilha de bastões, a visão computacional detecta cada face circular (1 face = 1 peça),
o operador confirma/corrige por toque na tela e o resultado é sincronizado com o backend.

Parte de um projeto acadêmico com 7 engenheiros. Backend em: `mgs-inventario-backend`.

## Stack

- **Kotlin** + arquitetura **MVVM**
- **CameraX** — captura guiada (enquadramento, foco, flash)
- **Jetpack Compose / Canvas** — conferência assistida por toque
- **Room (SQLite)** — persistência offline + fila de sincronização
- **TFLite** — detecção YOLO-nano rodando no próprio aparelho (on-device)

## Escopo do MVP

- Captura de foto única (bitola média/grande)
- Detecção on-device + confirmação por toque (adiciona/remove marcador)
- Operação **100% offline**, sincroniza quando houver rede
- Identificação do SKU pela peça (etiqueta MGS), nunca pela plaqueta da prateleira

Fora do MVP: soma de seções (stitching), medição de diâmetro por pixel, integração direta com o ERP.

## Como rodar

```bash
# Abrir no Android Studio (Giraffe ou superior)
# JDK 17, Android SDK 34

# Build via linha de comando:
./gradlew assembleDebug

# Instalar em dispositivo conectado:
./gradlew installDebug
```

Configure a URL do backend em `local.properties` (não versionado):

```properties
backend.base.url=http://SEU_IP:8000
```

## Estrutura (referência)

```
app/src/main/java/com/mgs/inventario/
├── camera/         # CameraX + captura guiada
├── conference/     # Canvas de marcadores + toque
├── data/           # Room (DAO, entidades, fila de sync)
├── domain/         # regras de negócio / casos de uso
├── ml/             # inferência TFLite
└── ui/             # telas Compose + ViewModels
```

## Convenções

- Branches: `feat/...`, `fix/...`, `chore/...`
- PR com pelo menos 1 revisão do squad antes do merge em `main`

> Plano de sprints e critérios de aceite do MVP: ver documento do projeto.
