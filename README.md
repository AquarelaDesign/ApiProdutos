# ApiProdutos
Criação de API com .NET Core 5.0

Projeto ASP .NET Core Web API

1. Criar entidade Produto (modelo anêmico)
2. Criar o contexto AppDbContext (mapeamento, configurações)
3. Definir a alimentação da tabela Produtos (HasData)
4. Definir string de conexão com o MySql no arquivo appsettings.json
5. Configurar o serviço no arquivo Startup (provedor, conexão)
6. Criar o Contoller ProdutosController via Scaffolding


Visual Studio 2019 (Community)

- Criar um projeto (C#, Todas as Plataformas, Web)
  1. API Web do ASP.NET Core

- Criar Pasta "Data"
  1. Adicionar -> Nova Pasta

- Definir Entidade de Dominio (dentro da pasta Data)
  1. Adicionar -> Novo Item -> Classe "Produto.cs"
      public int Id { get; set; }
      public string Nome { get; set; }
      public decimal Preco { get; set; }
      public int Estoque { get; set; }

- Ferramentas -> Gerenciador de Pacotes do NuGet -> Gerenciador Pacotes NuGet para a Solução...
  1. Procurar
  
    Microsoft.EntityFrameworkCore
    
    Microsoft.EntityFrameworkCore.Tools
    
    Microsoft.EntityFrameworkCore.Design
    
    Pomelo.EntityFrameworkCore.MySql
    
    Microsoft.AspNetCore.Authentication.JwtBearer
    
    Microsoft.AspNetCore.Authentication.OpenIdConnect

- Definir Classe de Contexto (dentro da pasta Data)

  1. Adicionar -> Novo Item -> Classe "AppDbContext.cs" (Mapeamento orm entre a entidade Produto e a tabela Produtos)

    ```
    public class AppDbContext : DbContext
    {
      public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
      {
      }

      public DbSet<Produto> Produtos { get; set; }

      protected override void OnModelCreating(ModelBuilder modelBuilder)
      {
         modelBuilder.Entity<Produto>()
            .Property(p => p.Nome)
            .HasMaxLength(80);

         modelBuilder.Entity<Produto>()
            .Property(p => p.Preco)
            .HasPrecision(10, 2);

         modelBuilder.Entity<Produto>()
            .HasData(
               new Produto { Id = 1, Nome = "Caderno", Preco = 7.95M, Estoque = 10},
               new Produto { Id = 2, Nome = "Borracha", Preco = 2.45M, Estoque = 30},
               new Produto { Id = 3, Nome = "Estojo", Preco = 6.25M, Estoque = 15});
      }
    }
    ```

- Definir configuração do Serviço no arquivo "Startup.cs"
  1. Incluir em "public void ConfigureServices"
    ```
    string mySqlConnection = Configuration.GetConnectionString("DefaultConnection");
    
    services.AddDbContextPool<AppDbContext>(options =>
      options.UseMySql(mySqlConnection,
      ServerVersion.AutoDetect(mySqlConnection)));
    ```

- Definir sessão "ConnectionStrings" no arquivo "appsettings.json"
    ```
    "ConnectionStrings": {
      "DefaultConnection": "server=localhost; port=3306; database=produtosdb; user=root; password=; Persist Security Info=False;"
    },
    ```
- Ferramentas -> Gerenciador de Pacotes do NuGet -> Console do Gerenciador de Pacotes

 1. Gerar o arquivo de Migration
 
   PM> `add-migration Inicial`

 2. Aplicar alterações no BD
 
   PM> `update-database`

- Criar API na Pasta "Controller"

  1. Adicionar -> Controlador -> API -> Controlador MVC com exibições, usando o Entity Framework
  
    Classe do modelo: "Produto"
    
    Classe de contexto de dados: "AppDbContext"
    
    Nome do controlador: ProdutosController
    

