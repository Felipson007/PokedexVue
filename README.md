---

# **Pokédex Interativa**

Uma Pokédex interativa que permite listar, buscar, filtrar e navegar entre Pokémon, utilizando Vue.js e Bootstrap. Este projeto consome a [PokéAPI](https://pokeapi.co/) para exibir informações sobre os Pokémon.

---

## **Requisitos**

Antes de começar, certifique-se de ter os seguintes itens instalados:

- [Node.js](https://nodejs.org/) (versão 16 ou superior)
- [npm](https://www.npmjs.com/) ou [yarn](https://yarnpkg.com/)

---

## **Passos para Configuração**

1. **Clone o Repositório**

   Clone o repositório para a sua máquina local:

   ```bash
   git clone https://github.com/seu-usuario/sua-pokedex.git
   cd sua-pokedex
   ```

2. **Instale as Dependências**

   Execute o comando para instalar as dependências necessárias:

   ```bash
   npm install
   ```

   ou, se preferir `yarn`:

   ```bash
   yarn install
   ```

3. **Execute o Servidor de Desenvolvimento**

   Após a instalação das dependências, inicie o servidor local:

   ```bash
   npm run serve
   ```

   ou, com `yarn`:

   ```bash
   yarn serve
   ```

4. **Abra no Navegador**

   Após iniciar o servidor, acesse a aplicação no navegador em:

   ```
   http://localhost:####
   ```

---

## **Funcionalidades**

- **Listagem de Pokémon**: Exibe uma lista de Pokémon obtidos da PokéAPI.
- **Busca**: Permite buscar Pokémon pelo nome.
- **Filtros**: Filtre Pokémon pelo tipo (ex.: fogo, água, elétrico).
- **Paginação**: Navegue entre diferentes páginas com os Pokémon.
- **Botão "Voltar ao Topo"**: Retorne ao topo da página com um clique.

---

## **Estrutura do Projeto**

```bash
src/
├── assets/               # Arquivos de mídia, como imagens e ícones
├── components/           # Componentes Vue reutilizáveis
│   ├── FilterBar.vue     # Componente de filtro por tipo
│   ├── Navbar.vue        # Barra de navegação
│   ├── Pagination.vue    # Componente de paginação
│   ├── PokemonCard.vue   # Cartão de exibição de Pokémon
│   ├── PokemonGrid.vue   # Grade de exibição de Pokémon
│   ├── ScrollToTopButton.vue # Botão flutuante para voltar ao topo
│   └── SearchBar.vue     # Barra de busca
├── views/                # Páginas completas (caso aplique rotas no futuro)
├── App.vue               # Componente raiz da aplicação
└── main.js               # Arquivo de inicialização da aplicação
```

---

## **Tecnologias Utilizadas**

- **Vue.js**: Framework principal para o desenvolvimento da interface.
- **Bootstrap**: Biblioteca de estilos para facilitar o design responsivo.
- **PokéAPI**: API pública usada para obter os dados dos Pokémon.

---

## **Customizações**

### Alterar o Limite de Pokémon

Por padrão, a aplicação exibe até 150 Pokémon. Para alterar, edite o método `fetchPokemons` no arquivo `App.vue`:

```javascript
const response = await fetch(`https://pokeapi.co/api/v2/pokemon?limit=150`);
```

Substitua `150` pelo número desejado.

---
