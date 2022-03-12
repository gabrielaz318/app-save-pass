### screens/Home/index.tsx

Essa é a página inicial do aplicativo. Aqui é feita a listagem dos dados de login que estão salvos no AsyncStorage e a busca de logins pelo título a partir da barra de busca. Aqui você irá implementar três funções:

- **loadData:** Essa função é responsável por buscar os dados no AsyncStorage e colocar nos estados `searchListData` e `data`.
    
    Os dados quando armazenados no AsyncStorage estarão em uma lista no seguinte formato: 
    
    ```tsx
    type LoginDataProps = Array<{
      id: string;
      title: string;
      email: string;
      password: string;
    }>;
    ```
    
    <aside>
    ⚠️ A key que você deverá usar para salvar e buscar os dados no AsyncStorage é `**"@savepass:logins"**`.
    Lembre-se de sempre usar essa key ou os testes não passarão corretamente.
    
    </aside>
    
    Após buscar os dados, eles devem ser salvos nos dois estados: `searchListData` e `data`.
    
- **handleChangeInputText:** Essa função irá receber o valor de texto digitado no input da barra de busca e colocar esse valor no estado `searchText`.

- **handleFilterLoginData:** Essa função deve buscar por logins no estado `data` que tenha o valor `searchText` incluída no `title`.
Ao filtrar esse dado, altere o estado `setSearchListData` com o resultado. Cuide para sempre manter o estado `data` com os dados originais. É importante que você também valide se o valor de `searchText` não é uma string vazia antes de realizar a filtragem.

### screens/RegisterLoginData/index.tsx

Nessa tela será feito o registro de novos dados de senhas a serem salvos pelo aplicativo. Aqui você precisará implementar apenas uma função:

- **handleRegister:** Essa função é responsável por receber os dados do formulário com o seguinte formato:
    
    ```tsx
    {
      service_name: string;
      email: string;
      password: string;
    }
    ```
    
    A função já está se encarregando de inserir um `id` para você. Então o formato da variável `newLoginData` terá o formato: 
    
    ```tsx
    {
    	id: string;
      service_name: string;
      email: string;
      password: string;
    }
    ```
    
    Após isso, você deve **adicionar** (lembre-se de manter os dados já salvos) o novo registro no AsyncStorage e fazer a navegação para a tela `Home`. 
    

- Além de implementar a função, será necessário que você adicione as regras de negócio com as mensagens de erro corretamente em cada Input para que o React Hook Form consiga exibi-los em caso de erro na validação.