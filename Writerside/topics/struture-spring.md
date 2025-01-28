# Estrutura de Pastas e Arquivos para Projetos Spring

Este documento descreve uma estrutura de pastas e arquivos recomendada para projetos Spring. A organização visa facilitar a manutenção, escalabilidade e entendimento do código.

---

## Estrutura Geral

```
project-name/
├── pom.xml                       # Arquivo de configuração do Maven
├── Dockerfile                    # Configuração de contêiner Docker
├── compose.yaml                  # Arquivo para o Docker Compose
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── companyname/
│   │   │           └── projectname/
│   │   │               ├── controller/       # Controladores REST
│   │   │               │   └── [Feature]Controller.java
│   │   │               ├── model/            # Representações de dados
│   │   │               │    ├── entityname/
│   │   │               │    |      ├── entity/       # Entidades JPA
│   │   │               │    |      │   └── [Feature].java
│   │   │               │    |      ├── module/       # DTOs
│   │   │               │    |      │   └── [Feature]Dto.java
│   │   │               │    |      ├── mapper/       # MapStruct
│   │   │               │    |      │   └── [Feature]Mapper.java
│   │   │               │    |      └── enums/        # Enumeradores
│   │   │               │    |           └── [Feature]Type.java
│   │   │               │    └── InterfaceMapper.java
│   │   │               ├── repository/       # Interfaces de repositório
│   │   │               │    └── [Feature]Repository.java
│   │   │               ├── service/          # Lógica de negócios
│   │   │               │   └── entityname/
│   │   │               │   |   ├── [Feature]Service.java
│   │   │               │   |   └── [FeatureEspecific]Service.java
│   │   │               │   └── InterfaceService.java
│   │   │               ├── config/           # Configurações
│   │   │               │   └── SecurityConfig.java
│   │   │               └── exception/        # Tratamento de erros
│   │   │                   ├── CustomException.java
│   │   │                   └── ExceptionHandler.java
│   │   ├── resources/
│   │   │   ├── application.properties        # Configuração padrão
│   │   │   ├── application-dev.properties    # Configuração para dev
│   │   │   ├── application-prod.properties   # Configuração para produção
│   │   │   ├── messages.properties           # Mensagens de internacionalização
│   │   │   └── data.sql                      # Dados iniciais do banco
│   │   └── webapp/                           # Arquivos estáticos
│   │       └── index.html
│   └── test/
│       └── java/
│           └── com/
│               └── companyname/
│                   └── projectname/
│                       └── controller/
                             └── [Feature]ControllerTest/
                                        ├──  [Method]Test.java 
                                        ├── [Method]SuccessTest.java
                                        ├── [Method]FailureTest.java 
                                        └── [Method]ExceptionTest.java

```
---
## Estrutura Visual

![structure-spring.png](struture-spring.png)

---

## Descrição das Pastas e Arquivos

### 1. `controller/` – Controladores REST

- **Função**: Responsável por expor os pontos de acesso da API.
- **Exemplo**:
    - **`[Feature]Controller.java`**: Controlador que mapeia as requisições HTTP para os serviços correspondentes.

### 2. `model/` – Representações de Dados

- **Função**: Contém as entidades JPA, DTOs, mapeadores e enumeradores do projeto.
- Subdivisões:
    - **`entityname/`**:
        - **`entity/`**: Entidades JPA que representam as tabelas do banco de dados.
        - **`module/`**: DTOs (Data Transfer Objects) para transferência de dados.
        - **`mapper/`**: Mapeadores que convertem entre as entidades e os DTOs. Utiliza o MapStruct para facilitar a conversão.
        - **`enums/`**: Enumeradores que definem tipos específicos para o projeto, como estados ou tipos de recursos.
        - **`InterfaceMapper.java`**: Interface comum para mapeamento entre entidades e DTOs.

### 3. `repository/` – Repositórios

- **Função**: Contém as interfaces que comunicam a aplicação com o banco de dados.
- **Exemplo**:
    - **`[Feature]Repository.java`**: Interface que estende `JpaRepository` e define métodos para manipulação de dados no banco.

### 4. `service/` – Lógica de Negócios

- **Função**: Contém a lógica de negócios da aplicação, que processa as requisições do controlador.
- Subdivisões:
    - **`[Feature]Service.java`**: Serviço responsável pela lógica de negócios de uma funcionalidade específica.
    - **`[FeatureEspecific]Service.java`**: Serviço específico para uma funcionalidade ainda mais granular ou específica.
    - **`InterfaceService.java`**: Interface com métodos comuns de CRUD, como `findAll()`, `findById()`, `save()`, e `delete()`.

### 5. `config/` – Configurações

- **Função**: Contém configurações de segurança, CORS, entre outras configurações do sistema.
- **Exemplo**:
    - **`SecurityConfig.java`**: Configurações de segurança da aplicação, como autenticação e autorização.

### 6. `exception/` – Tratamento de Erros

- **Função**: Classes responsáveis pelo tratamento de exceções e erros da aplicação.
- Subdivisões:
    - **`CustomException.java`**: Exceção personalizada para erros específicos do projeto.
    - **`ExceptionHandler.java`**: Tratador global de exceções.

### 7. `resources/` – Arquivos de Configuração e Internacionalização

- **Função**: Contém arquivos de configuração da aplicação e de internacionalização.
- Subdivisões:
    - **`application.properties`**: Configurações gerais do Spring Boot.
    - **`application-dev.properties`**: Configurações para o ambiente de desenvolvimento.
    - **`application-prod.properties`**: Configurações para o ambiente de produção.
    - **`messages.properties`**: Mensagens utilizadas para internacionalização.
    - **`data.sql`**: Dados iniciais para o banco de dados.

### 8. `webapp/` – Arquivos Estáticos

- **Função**: Contém os arquivos estáticos, como HTML, CSS e JavaScript.
- **Exemplo**:
    - **`index.html`**: Página principal da aplicação.

### 9. `test/` – Testes

- **Função**: Contém os testes unitários e de integração da aplicação.
- Subdivisões:
    - **`controller/`**: Testes para os controladores REST.
        - **`[Feature]ControllerTest/`**: Pasta para os testes de um controlador específico.
            - **`[Method]Test.java`**: Teste genérico para um método específico do controlador.
            - **`[Method]SuccessTest.java`**: Teste de sucesso para um método específico do controlador.
            - **`[Method]FailureTest.java`**: Teste de falha para um método específico do controlador.
            - **`[Method]ExceptionTest.java`**: Teste de exceção para um método específico do controlador.

---

## Exemplos de Código

### 1. Interface `InterfaceService.java` - Métodos Comuns de CRUD

```java
public interface InterfaceService<T, ID> {
    List<T> findAll();
    Optional<T> findById(ID id);
    T save(T entity);
    void deleteById(ID id);
}
```

### 2. Interface `InterfaceMapper.java` - Métodos Comuns de Conversão entre Entidade e DTO

```java
public interface InterfaceMapper<E, D> {
    D toDto(E entity);
    E toEntity(D dto);
    List<D> toDtoList(List<E> entities);
    List<E> toEntityList(List<D> dtos);
}
```

### 3. Exemplo de Serviço `FeatureService.java` - Implementação de `InterfaceService`

```java
@Service
public class FeatureService implements InterfaceService<FeatureDto, Long> {

    private final FeatureEspecificService featureEspecificService;

    public FeatureService(FeatureEspecificService featureEspecificService) {
        this.featureEspecificService = featureEspecificService;
    }

    @Override
    public List<FeatureDto> findAll() {
        return featureEspecificService.getAllFeatures();
    }

    @Override
    public Optional<FeatureDto> findById(Long id) {
        return featureEspecificService.findFeatureById(id);
    }

    @Override
    public FeatureDto save(FeatureDto featureDto) {
        return featureEspecificService.createFeature(featureDto);
    }

    @Override
    public void deleteById(Long id) {
        featureEspecificService.deleteFeatureById(id);
    }
}
```

### 4. Exemplo de Mapeador `FeatureMapper.java` - Implementação de `InterfaceMapper`

```java
@Component
public class FeatureMapper implements InterfaceMapper<Feature, FeatureDto> {

    @Override
    public FeatureDto toDto(Feature entity) {
        return new FeatureDto(entity.getId(), entity.getName(), entity.getType());
    }

    @Override
    public Feature toEntity(FeatureDto dto) {
        return new Feature(dto.getId(), dto.getName(), dto.getType());
    }

    @Override
    public List<FeatureDto> toDtoList(List<Feature> entities) {
        return entities.stream().map(this::toDto).collect(Collectors.toList());
    }

    @Override
    public List<Feature> toEntityList(List<FeatureDto> dtos) {
        return dtos.stream().map(this::toEntity).collect(Collectors.toList());
    }
}
```

---

## Benefícios da Estrutura Proposta

### 1. **Reutilização e Padronização**:
- A interface `InterfaceService` define os métodos comuns de CRUD, permitindo reutilização em todos os serviços e garantindo padronização na manipulação de dados.
- A interface `InterfaceMapper` facilita a conversão entre entidades e DTOs, reduzindo a repetição de código.

### 2. **Modularização**:
- A separação entre `Service` e `ServiceEspecific` permite que a lógica de negócios seja bem segmentada, facilitando a manutenção e a escalabilidade do projeto.
- O uso do MapStruct para mapeamento entre entidades e DTOs mantém a conversão de dados de forma clara e eficiente.

### 3. **Escalabilidade**:
- A estrutura modular facilita a expansão do sistema, permitindo a adição de novos módulos sem grandes mudanças na arquitetura existente.
- A interface `InterfaceService` e o uso do MapStruct garantem que novos recursos possam ser implementados de forma consistente.
