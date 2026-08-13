# Acesso Exclusivo (Escola de Cyber)

**Categoria:** Criptografia

## Introdução

Este desafio é uma questão de criptografia presente na plataforma da Escola de Cyber. O desafio consiste em analisar uma cifra fornecida em um arquivo de texto e, a partir de uma dica sobre a lógica do algoritmo usado para embaralhar os dados, identificar o método de criptografia empregado para então quebrá-lo e recuperar a flag escondida.

## Análise Inicial

O enunciado do desafio é o seguinte:

> A lógica do algoritmo invasor é simples: 'ou é uma coisa, ou é outra, mas nunca as duas'. Use esse conhecimento contra a própria cifra para quebrar a criptografia.
> Modelo de flag: FLAG{...}

Diferente de desafios de web que exigem lançar uma instância e acessar um site, aqui o desafio já nos entrega diretamente um arquivo de texto (`flag(4).txt`) contendo a cifra que precisamos quebrar:

```
25783566184c44533c5f47583c464742534247533c5945151649
```

Antes de sair tentando decifrar às cegas, vale reparar em dois detalhes que o próprio enunciado nos entrega de graça: a dica sobre a "lógica do algoritmo invasor", e o modelo de flag `FLAG{...}` — que vamos usar mais adiante como vantagem a nosso favor.

## Interpretação

A frase "ou é uma coisa, ou é outra, mas nunca as duas" é, na prática, a definição da porta lógica **XOR** (OU exclusivo): ela só retorna verdadeiro quando as duas entradas são diferentes entre si, nunca quando são iguais. Essa é a pista central de que o algoritmo usado para embaralhar a flag foi uma cifra XOR.

Além disso, reparando no conteúdo do arquivo, ele é composto só por caracteres de `0-9` e `a-f`, um forte indício de que o texto está representado em **hexadecimal**, e não é a cifra "crua" em bytes.

## Resolução

Para resolver, usamos a ferramenta online [CyberChef](https://cyberchef.org), que permite montar uma "receita" (Recipe) encadeando operações sobre o texto de entrada.

**1. Convertendo de hexadecimal**

Colamos a cifra no campo de Input e adicionamos a operação **"From Hex"** com o Delimiter em "Auto", convertendo a string hexadecimal para os bytes reais da cifra.

**2. Aplicando XOR com a flag conhecida como chave**

Em seguida, adicionamos a operação **"XOR"**. Como sabemos que a flag deve começar com `FLAG{` (informado no próprio enunciado), colocamos `FLAG{` no campo **Key** (com o esquema UTF8). Isso funciona porque a operação XOR é reversível: se `cifra = texto ⊕ chave`, então `texto ⊕ cifra = chave` — ou seja, ao "XORar" a cifra com o texto que ela deveria começar, o resultado nos mostra a própria chave usada na criptografia, como mostra a imagem abaixo:

<img width="1442" height="593" alt="image" src="https://github.com/user-attachments/assets/e0b44ebf-d8e0-497c-95f1-d75f6b8a979f" />


O Output mostrou o seguinte:

```
c4t!c
```

*(seguido de caracteres não imprimíveis/lixo, já que só os 5 primeiros bytes da cifra foram XORados corretamente com a chave)*

Repare que o 5º caractere (`c`) repete o 1º (`c`) — esse é o sinal de que a chave real tem apenas 4 caracteres e se repete ao longo de toda a cifra: `c4t!`.

**3. Decifrando com a chave correta**

Trocamos o valor do campo **Key** da operação XOR de `FLAG{` para `c4t!`. Como o CyberChef repete a chave automaticamente para cobrir todo o comprimento do texto, o Output já mostra a flag decifrada por completo, como visto na imagem a seguir:

<img width="1423" height="700" alt="image" src="https://github.com/user-attachments/assets/7af9ef7e-8fb0-4bcb-bf9b-e2b5297ae606" />


```
FLAG{x0r_k3y_r3c0v3r_m14u}
```

## Flag

```
FLAG{x0r_k3y_r3c0v3r_m14u}
```

## Conclusão

Este desafio aborda um erro clássico na implementação de criptografia XOR: usar uma **chave curta e repetida** em vez de uma chave do tamanho da mensagem, usada uma única vez (o verdadeiro *one-time pad*). Quando a chave se repete, basta conhecer — ou até apenas *adivinhar*, como no nosso caso, aproveitando o formato previsível `FLAG{...}` — um pequeno trecho do texto original para recuperar a chave inteira e decifrar toda a mensagem. Fica o aprendizado de que XOR só é realmente seguro quando usado como *one-time pad* de verdade: chave aleatória, do mesmo tamanho da mensagem, e nunca reutilizada.
