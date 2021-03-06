# ava-01-oo-abstracao-construcao-estado-comportamento

Classroom assignment <https://classroom.github.com/a/Ogoqb292>

## Modelar e implementar respeitando os princípios básicos de Orientação a Objetos

#### Assuntos

- Abstrações
- Classes
- [Objetos](http://youtu.be/c9NaVWIRT0A?t=93)
- Construtores
- Validade
- Atributos
- Estado
- Comportamento
- Mutabilidade
- Métodos comando e consulta
- Especificações

#### Prazo: 2019-03-31 Peso: 0.9 pts

**Restrição: não podem ser usadas as bibliotecas do Java, por exemplo, a classe `Math`, `Scanner`, etc, inclusive os métodos de Integer, como `parseInt` ou métodos de String, EXCETO `length`, `charAt` e `equals`; Os Casos de Teste podem ser corrigidos, mas a especificação não pode ser alterada.**

### Implementar Máquina D'água (0.3 pts)

Considere um máquina de água sofisticada. Ela é abastecida com uma Bombona de _20L_ e sempre que isso é feito um botão _abastecer água_ é pressionado, efetuando o _reset_ do contador para _20L_ (ou _20000mL_) disponíveis. A máquina também armazena internamente os copos descartáveis, de _200mL_ e _300mL_, com um repositório de 100 unidades para cada. Feito os abastecimentos, os usuários servem-se de água pressionando os botões que servem _200mL_ ou _300mL_. A máquina automaticamente saca um copo e o enche de água. A máquina mostra no painel a quantidade de água e copos disponíveis. Quando um botão _servir_ é pressionado e não há água ou copo, naturalmente o pedido não é atendido.

Dada esta especificação nosso analista projetou a seguinte interação conforme os Casos de Teste a seguir:

```java
MaquinaAgua maq = new MaquinaAgua();
System.out.println(maq.agua() == 0); // mL
System.out.println(maq.copos200() == 0);
System.out.println(maq.copos300() == 0);

maq.servirCopo200(); // não efetua, pois não há copo nem água

System.out.println(maq.agua() == 0); // mL
System.out.println(maq.copos200() == 0); // unidades
System.out.println(maq.copos300() == 0); // unidades

maq.abastecerAgua(); // inicializa 20000mL
System.out.println(maq.agua() == 20000); // mL

maq.abastecerAgua(); // mantém consistente
System.out.println(maq.agua() == 20000); // mL

maq.servirCopo200(); // não efetua, sem copo
System.out.println(maq.agua() == 20000); // mL

System.out.println(maq.copos200() == 0);
maq.abastecerCopo200(); // agora a máquina possui 100 copos de 200
System.out.println(maq.copos200() == 100);

maq.servirCopo200(); // -200
maq.servirCopo200(); // -200
maq.servirCopo200(); // -200
maq.servirCopo200(); // -200
maq.servirCopo200(); // -200, isto é, -1000ml e 5 copos de 200

System.out.println(maq.agua() == 19000);
System.out.println(maq.copos300() == 0);
System.out.println(maq.copos200() == 95);
maq.abastecerCopo200(); // agora a máquina possui 100 copos de 200 novamente
System.out.println(maq.copos200() == 100);

maq.servirCopo200(); // -200ml e 1 copo de 200
System.out.println(maq.agua() == 18800);
System.out.println(maq.copos200() == 99);

System.out.println(maq.copos300() == 0);
maq.servirCopo300(); // não efetua, não há copo 300
maq.abastecerCopo300(); // agora a máquina possui 100 copos de 300
System.out.println(maq.copos300() == 100);
maq.servirCopo300(); // agora efetua
System.out.println(maq.agua() == 18500);
System.out.println(maq.copos200() == 99);
System.out.println(maq.copos300() == 99);

// servir 50 copos de 300 = -15000ml
for (int i = 0; i < 50; i++) maq.servirCopo300();

System.out.println(maq.agua() == 3500);
System.out.println(maq.copos200() == 99);
System.out.println(maq.copos300() == 49);

// servir 17 copos de 200 = 3400ml
for (int i = 0; i < 17; i++) maq.servirCopo200();

System.out.println(maq.agua() == 100);
System.out.println(maq.copos200() == 82);
System.out.println(maq.copos300() == 49);

// não há água para atender o pedido
maq.servirCopo300();
System.out.println(maq.agua() == 100);
System.out.println(maq.copos200() == 82);
System.out.println(maq.copos300() == 49);

// não há água para atender o pedido
maq.servirCopo200();
System.out.println(maq.agua() == 100);
System.out.println(maq.copos200() == 82);
System.out.println(maq.copos300() == 49);

maq.abastecerAgua(); // inicializa 20000mL
System.out.println(maq.agua() == 20000);
System.out.println(maq.copos200() == 82);
System.out.println(maq.copos300() == 49);

// servir os 49 copos de 300 restantes = 14700ml
while (maq.copos300() > 0) maq.servirCopo300();

System.out.println(maq.agua() == 5300);
System.out.println(maq.copos200() == 82);
System.out.println(maq.copos300() == 0);

// não há copo para atender o pedido
maq.servirCopo300();
System.out.println(maq.agua() == 5300);
System.out.println(maq.copos200() == 82);
System.out.println(maq.copos300() == 0);

maq.servirCopo200(); // de 200 ok
maq.servirCopo200(); // de 200 ok

System.out.println(maq.agua() == 4900);
System.out.println(maq.copos200() == 80);
System.out.println(maq.copos300() == 0);

maq.abastecerCopo300(); // 100 copos de 300
System.out.println(maq.agua() == 4900);
System.out.println(maq.copos200() == 80);
System.out.println(maq.copos300() == 100);

// 3 copos de 300
maq.servirCopo300(); maq.servirCopo300(); maq.servirCopo300();

System.out.println(maq.agua() == 4000);
System.out.println(maq.copos200() == 80);
System.out.println(maq.copos300() == 97);
```

### Implementar Forno (0.3 pts)

Considere um Forno sofisticado de controle via app Android/iOS. É possível ligar, desligar, ajustar temperatura e outros detalhes. Os modelos variam segundo seu volume, tensão, potência e dimensões (na forma largura, altura e profundidade em cm). Então, implemente conforme especificação a seguir.

Casos de Teste:
```java
Forno f = new Forno(45, 220, 1700, 66, 40, 54);
System.out.println(f.volume == 45);
System.out.println(f.tensao == 220);
System.out.println(f.potencia == 1700);
System.out.println(f.largura == 66);
System.out.println(f.altura == 40);
System.out.println(f.profundidade == 54);
// todos esses atributos devem ser constantes,
// as atribuções a seguir não podem compilar
// verifique e comente-as
f.volume = 450;
f.tensao = 2200;
f.potencia = 17000;
f.altura = 400;
f.largura = 660;
f.profundidade = 540;
// ---
// ---
// PATCH
// Forno forno = new Forno(84, 220, 1860, 58, 61, 58);
Forno forno = new Forno(84, 220, 1860, 61, 58, 58);
System.out.println(forno.volume = 84);
System.out.println(forno.tensao = 220);
System.out.println(forno.potencia = 1860);
System.out.println(forno.altura = 58);
System.out.println(forno.largura = 61);
System.out.println(forno.profundidade = 58);

// métodos para consulta
System.out.println(forno.temperatura()); // 0 (de 50 a 300)
System.out.println(forno.ligado()); // false
// os atributos temperatura e ligado devem ser inacessíveis
// não deve compilar, verifique e depois comente as seguintes linhas
System.out.println(forno.temperatura);
System.out.println(forno.ligado);
// ----- temperatura deve
System.out.println(forno.ligado() == false);
forno.aumentarTemperatura(); // liga e vai para 50
System.out.println(forno.ligado() == true);
System.out.println(forno.temperatura() == 50); // 50
forno.aumentarTemperatura();
System.out.println(forno.temperatura() == 100); // 100
forno.aumentarTemperatura();
System.out.println(forno.temperatura() == 150); // 150
forno.aumentarTemperatura();
System.out.println(forno.temperatura() == 200); // 200
forno.aumentarTemperatura();
System.out.println(forno.temperatura() == 220); // 220
forno.aumentarTemperatura();
System.out.println(forno.temperatura() == 250); // 250
forno.aumentarTemperatura();
System.out.println(forno.temperatura() == 300); // 300

forno.aumentarTemperatura(); // está no máximo
System.out.println(forno.temperatura() == 300); // 300
System.out.println(forno.ligado() == true);
// reduzindo
forno.diminuirTemperatura();
forno.diminuirTemperatura();
forno.diminuirTemperatura();
// PATCH
// System.out.println(forno.temperatura() == 200); // 200
System.out.println(forno.temperatura() == 150); // 150
// desligando direto
forno.desligar();
System.out.println(forno.ligado() == false);
System.out.println(forno.temperatura() == 0);
// já está desligado
forno.diminuirTemperatura();
System.out.println(forno.ligado() == false);
System.out.println(forno.temperatura() == 0);

// timer de 1 a 120 minutos
forno.setTimer(15); // minutos
forno.aumentarTemperatura();
forno.aumentarTemperatura();
forno.aumentarTemperatura();
System.out.println(forno.ligado() == true);
System.out.println(forno.temperatura() == 150);
System.out.println(forno.tempoRestante() == 15);
forno.tick(); // tick do timer (baixa 1min)
System.out.println(forno.tempoRestante() == 14);
forno.tick(); forno.tick(); forno.tick(); forno.tick();
System.out.println(forno.tempoRestante() == 10);
System.out.println(forno.ligado() == true);
System.out.println(forno.temperatura() == 150);
forno.tick(); forno.tick(); forno.tick(); forno.tick(); forno.tick();
forno.tick(); forno.tick(); forno.tick(); forno.tick(); forno.tick();
System.out.println(forno.tempoRestante() == 0);
System.out.println(forno.ligado() == false);
System.out.println(forno.temperatura() == 0);
// novo timer
forno.setTimer(120);
forno.aumentarTemperatura(); forno.aumentarTemperatura();
System.out.println(forno.ligado() == true);
// PATCH
// System.out.println(forno.temperatura() == 150);
System.out.println(forno.temperatura() == 100);
System.out.println(forno.tempoRestante() == 120);
while (forno.ligado()) forno.tick();
System.out.println(forno.tempoRestante() == 0);
System.out.println(forno.ligado() == false);
System.out.println(forno.temperatura() == 0);
```

### Modelar e implementar TV (0.2 pts)

Considere um aparelho de televisão. Cada uma têm um fabricante, modelo, tamanho e resolução. Além disso, a operação da TV é bem simples, permitir aumentar e baixar o volume, numa escala de 0 a 100%, e mudar o canal, suportando apenas a faixa UHF, de 2 a 69.

Dada essa especificação, modele, projete e implemente uma classe `TV`, que guarde as características mencionadas, respeitando a imutabilidade e os métodos comandos e consultas que representem as operações descritas. Escreva pelo menos 20 Casos de Teste, para situações comuns e excepcionais.

Desafio: implementar o _mudo_, _ir para canal_ e _voltar canal_ (não obrigatório).

### Especificar um Objeto (0.1 pts)

Escreva uma especificação textual descrevendo as características e operações de um objeto. Pode ser físico, tangível, como a TV e Caneta, ou não, como a Conta Corrente, Fração. Após, faça a especificação operacional escrevendo os casos de teste.

**Não é para implementar, só especificar.**

Espera-se alguns (mais de um) atributos e métodos comando e consulta. Escreva quantos testes forem necessários para cobrir o caminho feliz e as situações excepcionais, quebrando ou não, escolha sua.

* * *

> _"First, solve the problem.
> Then, write the code."_
>
> -- **John Johnson**
