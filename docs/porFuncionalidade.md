### Carregando informações salvas no AsyncStorage

Para que o app consiga mostrar uma listagem de todas as senhas salvas no AsyncStorage, o único arquivo a ser editado será o `screens/Home/index.tsx`.

Aqui, iremos nos concentrar apenas na função `loadData` por enquanto. Já que apenas ela é quem faz o carregamento das informações que precisamos para essa funcionalidade. Essa função será chamada no `useFocusEffect` **sempre** que o componente `Home` estiver em exibição.

Você precisará buscar as informações do AsyncStorage usando a key `"@savepass:logins"` que já está na variável `datakey` e após isso, verificar se existe algum conteúdo no resultado da busca. Caso exista, adicione essa informação nos dois estados `data` e `searchListData`.

⚠ Lembre-se de usar o `JSON.parse` antes de adicionar as informações no estado, já que elas virão no formato de string do AsyncStorage. 

*Porque eu preciso duplicar os dados em dois estados?*

Faremos isso para que quando o usuário buscar por alguma senha na barra de busca, manteremos sempre o estado `data` com todas as informações que estão no AsyncStorage e o estado `searchListData` com o resultado da busca (filtrado com base na palavra digitada pelo usuário e os dados em `data`). Como no render do componente ainda não foi feita nenhuma busca, então `data` inteiro deverá ser exibido e não apenas parte dele. 

### Implementando a barra de busca

Ainda no arquivo `screens/Home/index.tsx` iremos implementar as seguintes funções:

- **handleChangeInputText:** Essa função irá receber o valor digitado pelo usuário e adicionará no estado `searchText`. 
Além disso, você precisará verificar se o valor do input não é uma string vazia `''`, caso seja, significa que nenhuma busca está sendo feita e o estado `searchListData` precisará conter todo o estado `data`.
- **handleFilterLoginData:** Essa função será chamada assim que o usuário clicar no botão de busca da barra de pesquisa. Ela vai modificar o estado `searchListData` de acordo com o que o usuário digitou no input.
Para isso, você precisa antes verificar se o input possui uma string válida, realizar a filtragem dos dados de senha pelo `service_name` e então adicionar ao estado `searchListData`.

### Adicionando novas senhas

Para essa funcionalidade, editaremos o arquivo `screens/RegisterLoginData/index.tsx`. Dentro dele há uma função:

- **handleRegister:** Essa função irá receber os dados de uma nova senha de acordo com o valor dos inputs e validar os valores digitados com o `schema` declarado logo acima do componente.
Na função, um `id` já é adicionado ao objeto com os dados da senha para que o React não reclame sobre isso ao fazermos a listagem na tela `Home`.
    
    Tudo que você precisa fazer aqui é buscar por todos os dados que já estão salvos no AsyncStorage, adicionar o novo dado na lista, salvar de volta no AsyncStorage e navegar de volta para a tela `Home`. Lembre de usar o método `JSON.parse` para converter os dados ao buscar no banco e `JSON.stringify` antes de salvar de volta para garantir que os dados estarão no formato correto.
    

Além de completar o código da função, você precisa também corrigir as mensagens de erro a serem mostradas nos inputs caso alguma validação do campo não passe. Para isso, use o objeto `errors` provido pelo `useForm` como visto durante as aulas.
Exemplo: 

```tsx
//useForm
const {
    control,
    handleSubmit,
    formState: {
      errors // objeto com as mensagens de erros
    }
  } = useForm({
    resolver: yupResolver(schema)
  });

// Antes
<Input
	testID="service-name-input"
	title="Nome do serviço"
	name="service_name"
	error={
	  // Replace here with real content
	  'Has error ? show error message'
	}
	control={control}
	autoCapitalize="sentences"
	autoCorrect
/>

// Depois
<Input
	testID="service-name-input"
	title="Nome do serviço"
	name="service_name"
	error={
	  errors.service_name && errors.service_name.message
	}
	control={control}
	autoCapitalize="sentences"
	autoCorrect
/>
```