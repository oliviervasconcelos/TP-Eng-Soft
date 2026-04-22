# Projeto Arquitetural e Modelagem UML --- Sistema de Evento Esportivo

------------------------------------------------------------------------

## 🏗️ Projeto Arquitetural --- Diagrama de Componentes

``` mermaid
flowchart LR

User[Usuário]
Admin[Administrador]

subgraph Web
    UI[Interface Web]
end

subgraph Application Layer
    GameController
    TeamController
    CityController
    BracketController
    AdminController
end

subgraph Domain Layer
    GameService
    TeamService
    CityService
    BracketService
    AuthService
end

subgraph Infrastructure Layer
    Database[(Banco de Dados)]
    Repository
end

subgraph External
    API[API Esportiva Externa]
end

User --> UI
Admin --> UI

UI --> GameController
UI --> TeamController
UI --> CityController
UI --> BracketController
UI --> AdminController

GameController --> GameService
TeamController --> TeamService
CityController --> CityService
BracketController --> BracketService
AdminController --> AuthService

GameService --> Repository
TeamService --> Repository
CityService --> Repository
BracketService --> Repository

Repository --> Database

GameService --> API
```

------------------------------------------------------------------------

## 🎯 Diagrama de Casos de Uso

``` mermaid
flowchart LR

User((Usuário))
Admin((Administrador))

User --> ViewGames[Visualizar tabela de jogos]
User --> FilterGames[Filtrar partidas]
User --> ViewTeams[Consultar seleções]
User --> ViewCities[Consultar cidades-sede]
User --> ViewBracket[Visualizar chaveamento]
User --> ViewMatch[Ver detalhes da partida]

Admin --> Login[Autenticar]
Admin --> UpdateResult[Atualizar resultados]
Admin --> DefineQualified[Definir classificados]
```

------------------------------------------------------------------------

## 🧠 Diagrama de Classes

``` mermaid
classDiagram

class Usuario {
    +id
    +nome
}

class Administrador {
    +login()
    +atualizarResultado()
}

Usuario <|-- Administrador

class Selecao {
    +id
    +nome
    +grupo
    +tecnico
}

class CidadeSede {
    +nome
    +pais
}

class Estadio {
    +nome
    +capacidade
}

CidadeSede "1" --> "1" Estadio

class Partida {
    +id
    +data
    +horario
    +resultado
}

Selecao "2" --> "0..*" Partida

class Chaveamento {
    +fase
    +gerarChaveamento()
}

Partida --> Chaveamento

class Hotel {
    +nome
}

class Aeroporto {
    +nome
}

CidadeSede --> Hotel
CidadeSede --> Aeroporto
```

------------------------------------------------------------------------

## 🔄 Diagrama de Sequência --- Atualizar Resultado

``` mermaid
sequenceDiagram

Administrador->>UI: inserir resultado
UI->>AdminController: atualizarResultado()
AdminController->>AuthService: validarLogin()
AuthService-->>AdminController: OK

AdminController->>GameService: atualizarResultado()
GameService->>Repository: salvarResultado()
Repository->>Database: update

Database-->>Repository: OK
Repository-->>GameService: OK
GameService-->>UI: resultado atualizado
```

------------------------------------------------------------------------

## 📦 Diagrama de Pacotes

``` mermaid
flowchart TB

subgraph presentation
    ui
end

subgraph application
    controllers
end

subgraph domain
    entities
    services
end

subgraph infrastructure
    repository
    database
    api
end

ui --> controllers
controllers --> services
services --> entities
services --> repository
repository --> database
services --> api
```
