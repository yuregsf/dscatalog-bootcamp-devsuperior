# DevSuperior Bootcamp

Estudo de Spring e React realizado no Bootcamp DevSuperior.

## Spring

Anotações realizadas ao longo do bootcamp considerando assuntos que já conheço e portanto não terá explicações longas.

### Criar projeto

Primeiro definir o worskapce no STS. Dentro do workspace criar a pasta do projeto e descompactar o arquivo gerado pelo (https://start.spring.io/)[spring initializr]. As configurações deste projeto seguem abaixo:

 - Project: Maven Project
 - Spring Boot: 2.3.13
 - Project Metadata:
    - Packaging: Jar
    - Java: 11
 - Depedencies:
    - Spring Web

Com isso já é possível rodar o projeto.

### Camadas

O projeto é dividido em camadas:
 
 - Controllers/Resouce (conversa com os services através de DTOs)
 - Services (Implementa funcionalidades utilizando repository)
 - Repository (Cria um repository da entity)
 - Entites

É bastante parecido com o modelo utilizado no NestJS.

#### Controller

Camada responsável por controlar as requisições. É a interface da API com o client.

**Exemplo GET**

 ```java
 @GetMapping
 public ResponseEntity<List<CategoryDTO>> findAll(){
    List<CategoryDTO> list = service.findAll();
    return ResponseEntity.ok().body(list);
 }
```

`@GetMapping` é a Annotation que define o tipo da rota, no caso GET. O retorno do método indica o código da resposta e o corpo.

#### Service

Camada reponsável por implementar as funcionalidades e utiliza o repository para se comunicar com o banco.

**Exemplo**

```java
@Autowired
private CategoryRepository repository;

@Transactional(readOnly = true)
   public List<CategoryDTO> findAll(){
      List<Category> list = repository.findAll();

      return list.stream().map(x -> new CategoryDTO(x)).collect(Collectors.toList());
   }
```

`@Autowired` diz para o spring resolver as depedências. 

`@Transactional` descreve o método como uma transação e neste exemplo indica que é um método só de leitura.

O método utiliza o método do repository para pegar todos os dados da entidade no banco de dados. Após isso mapeia o resultado para o formato do DTO.

#### Repository

Basicamente é uma classe que representa uma entidade e seus métodos do banco de dados.

#### Entity

É a entidade em si. Nela é possível definir as colunas e as relações com outras entidades.

### Outros detalhes

Como este é um projeto maven, suas dependências são declaradas no arquivo `pom.xml` na raiz do projeto.

O arquivo `application.properties` pode ser entendido como um `.env` do Node.
