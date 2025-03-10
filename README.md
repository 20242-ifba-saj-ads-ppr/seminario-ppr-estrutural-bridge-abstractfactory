# Bridge Padrão: Separando Abstração da Implementação

## Motivação
O padrão de projeto **Bridge** permite separar uma abstração de sua implementação, possibilitando que ambas evoluam independentemente. Isso promove um código mais flexível e com baixo acoplamento.

Neste exemplo, utilizamos o **Bridge** para enviar notificações por diferentes meios, como **E-mail** e **SMS**, mantendo a separação entre o tipo de mensagem e a forma de envio.

## UML do Bridge

```mermaid
classDiagram
    class MensagemImplementacao {
        +enviarMensagem(String mensagem)
    }
    class EmailMensagem {
        +enviarMensagem(String mensagem)
    }
    class SmsMensagem {
        +enviarMensagem(String mensagem)
    }
    class Mensagem {
        #MensagemImplementacao implementacao
        +enviar(String mensagem)
    }
    class MensagemSimples {
        +enviar(String mensagem)
    }
    class MensagemUrgente {
        +enviar(String mensagem)
    }
    
    MensagemImplementacao <|-- EmailMensagem
    MensagemImplementacao <|-- SmsMensagem
    Mensagem <|-- MensagemSimples
    Mensagem <|-- MensagemUrgente
    Mensagem --> MensagemImplementacao
```

## Código do Bridge

### Implementação - Interface para diferentes formas de envio de mensagem
```java
package bridge;

public interface MensagemImplementacao {
    void enviarMensagem(String mensagem);
}
```

### Implementações concretas

#### EmailMensagem
```java
package bridge;

public class EmailMensagem implements MensagemImplementacao {
    @Override
    public void enviarMensagem(String mensagem) {
        System.out.println("Enviando email: " + mensagem);
    }
}
```

#### SmsMensagem
```java
package bridge;

public class SmsMensagem implements MensagemImplementacao {
    @Override
    public void enviarMensagem(String mensagem) {
        System.out.println("Enviando SMS: " + mensagem);
    }
}
```

### Abstração - Interface para tipos de mensagens

#### Mensagem
```java
package bridge;

public abstract class Mensagem {
    protected MensagemImplementacao implementacao;
    
    public Mensagem(MensagemImplementacao implementacao) {
        this.implementacao = implementacao;
    }
    
    public abstract void enviar(String mensagem);
}
```

### Abstrações refinadas

#### MensagemSimples
```java
package bridge;

public class MensagemSimples extends Mensagem {
    public MensagemSimples(MensagemImplementacao implementacao) {
        super(implementacao);
    }

    @Override
    public void enviar(String mensagem) {
        implementacao.enviarMensagem("Mensagem Simples: " + mensagem);
    }
}
```

#### MensagemUrgente
```java
package bridge;

public class MensagemUrgente extends Mensagem {
    public MensagemUrgente(MensagemImplementacao implementacao) {
        super(implementacao);
    }

    @Override
    public void enviar(String mensagem) {
        implementacao.enviarMensagem("*** URGENTE *** " + mensagem);
    }
}
```

### Implementação do Cliente (Main)
```java
package bridge;

public class Main {
    public static void main(String[] args) {
        Mensagem mensagemEmail = new MensagemSimples(new EmailMensagem());
        mensagemEmail.enviar("Olá, este é um teste de email.");
        
        Mensagem mensagemSms = new MensagemUrgente(new SmsMensagem());
        mensagemSms.enviar("Este é um alerta crítico!");
    }
}
```


## Explicação do Código
1. **Criamos a interface `MensagemImplementacao`**, que representa diferentes formas de envio de mensagem.
2. **Implementamos `EmailMensagem` e `SmsMensagem`**, que enviam mensagens por e-mail e SMS.
3. **Criamos a abstração `Mensagem`**, que mantém uma referência para uma implementação (`MensagemImplementacao`).
4. **Criamos as classes `MensagemSimples` e `MensagemUrgente`**, que refinam a abstração e adicionam lógica específica.
5. **No cliente (`Main`)**, combinamos diferentes abstrações e implementações de forma flexível.


## Participantes

- **Implementação (`MensagemImplementacao`)**
  - Define a interface para diferentes formas de envio de mensagens.

- **Implementações Concretas (`EmailMensagem`, `SmsMensagem`)**
  - Implementam o envio específico das mensagens.

- **Abstração (`Mensagem`)**
  - Define uma interface genérica para envio de mensagens, delegando o envio à implementação concreta.

- **Abstrações Refinadas (`MensagemSimples`, `MensagemUrgente`)**
  - Estendem `Mensagem` para oferecer diferentes tipos de mensagens.

- **Cliente (`Main`)**
  - Usa as abstrações e implementações de maneira independente, garantindo flexibilidade e escalabilidade.

