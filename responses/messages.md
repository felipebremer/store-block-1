# Práticas de internacionalização no VTEX IO

## Introdução
Com o bloco customizado na loja, devemos aprender a **internacionalizar** o conteúdo apresentado. 

É importante lembrar que os blocos devem sempre seguir boas práticas de localização, e **não devem mostrar strings _hardcoded_**, mas sim sensíveis a linguagem que a loja opera.

Não se preocupe, você não precisará adicionar traduções de todos os textos para as variadas linguagens nas quais o Store Framework é usado. Portanto, nessa etapa, serão apresentados conceitos acerca da internacionalização de *apps* e como fazê-la. 

## As *Messages*
O conceito de *messages* facilita a adição de novos idiomas ao tema. As *messages* centralizam todos os serviços de tradução na plataforma. Dada um texto a ser traduzido, *Messages* irá primeiramente checar o contexto definido pelo usuário para, em seguida, checar as traduções das *apps* e, por fim, passa pelo sistema de tradução automática.

Na estrutura do diretório, é possível observar que há uma pasta chamada `messages`, que apresenta três arquivos principais: `pt.json`, `en.json` e `es.json`, cada um responsável pelas traduções: português, inglês e espanhol, respectivamente. Além disso, a fim de fornecer traduções automáticas melhores, é utilizado o arquivo `context.json`, responsável por evitar ambiguidades.

Para utilizar tais definições, os arquivos de tradução mencionados anteriormente são JSON, cujas chaves são as mensagens e os valores são as traduções.

>O arquivo `context.json` é necessário e precisa conter todas as mensagens, além de oferecer as traduções automáticas em casos excepcionais.

## Atividade
Você já deve ter aprendido a usar o nosso **builder _messages_**, e será através dele que serão adicionadas *strings* internacionalizadas nos componentes. 

1. Para isso, **na pasta _messages_**, adicione agora uma mensagem de **título para o componente**:

    `messages/pt.json`
    ```diff
    {
      ...,
    +	"countdown.title": "Contagem Regressiva"
    }
    ```
    `messages/en.json`
    ```diff
    {
      ...,
    +	"countdown.title": "Countdown"
    }
    ```
    `messages/es.json`
    ```diff
    {
      ...,
    +	"countdown.title": "Cuenta Regresiva"
    }
    ```
    `messages/context.json`
    ```diff
    {
      ...,
    +	"countdown.title": "Countdown"
    }
    ```

2. Feito isso, para **renderizar o título** deve-se usar o componente `FormattedMessage` da biblioteca [react-intl](https://github.com/formatjs/react-intl).
    > A biblioteca _react-intl_ dá suporte a várias maneiras de configuração e internacionalização, vale a pena verificá-las.

3. Adicione a biblioteca usando `yarn add react-intl` na pasta *react*
4. No código do seu componente `Countdown.tsx` **importe o FormattedMessage**
    ```diff
    +	import { FormattedMessage } from 'react-intl'
    ```
5. Adicione uma constante que será o seu título:
    ```tsx
    const titleText = title || <FormattedMessage id="countdown.title"/>
    ```
6. Agora, junte o título e o contador para renderizá-los. Para isso, defina um container por fora. Além disso, o texto do título será passado através da *prop* `title`:
    ```tsx
    const Countdown: StorefrontFunctionComponent<CountdownProps> = ({ title, targetDate }) => {
      const [
        timeRemaining, 
        setTime
      ] = useState<TimeSplit>({
        hours: '00', 
        minutes: '00', 
        seconds: '00'
      })
      
      const titleText = title || <FormattedMessage id="countdown.title" /> 
      const handles = useCssHandles(CSS_HANDLES)

      tick(targetDate, setTime)

      return (
        <div className={`${handles.container} t-heading-2 fw3 w-100 c-muted-1`}>
          <div className={`${handles.title} db tc`}>
            { titleText }
          </div>
          <div className={`${handles.countdown} db tc`}>
            {`${timeRemaining.hours}:${timeRemaining.minutes}:${timeRemaining.seconds}`}
          </div>
        </div>
      )
    }
    ```
    Note que são utilizados três *handles* **novos**: *container*, *countdown* e *title*. Dessa forma, lembre-se de declará-los na constante `CSS_HANDLES`, vista na etapa anterior:
    ```tsx
    const CSS_HANDLES = ['container', 'countdown', 'title']
    ```

7. Por fim, é preciso adicionar a *prop* de `title` no *schema*:
    ```diff
    Countdown.schema = {
      title: 'editor.countdown.title',
      description: 'editor.countdown.description',
      type: 'object',
      properties: {
    +    title: {
    +      title: 'Sou um título',
    +      type: 'string',
    +      default: null,
    +    },
        targetDate: {
          title: 'Sou um título',
          description: 'Sou uma descrição',
          type: 'string',
          default: null,
        },
      },
    }
    ```

Pronto! Agora, para testar sua loja em outros idiomas basta adicionar a *query string* `/?cultureInfo=pt-br` na URL, por exemplo. Ao utilizar tal URL, o resultado esperado é esse aqui:

![image](https://user-images.githubusercontent.com/19495917/75484759-23d7f000-5988-11ea-8b0a-63a5fce4ea7e.png)