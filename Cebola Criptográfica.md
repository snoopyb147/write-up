# Cebola Criptográfica (Escola de Cyber)

**Categoria:** Criptografia / Codificação

## Introdução

Este desafio é uma questão de criptografia presente na plataforma da Escola de Cyber. O nome "Cebola Criptográfica" já é uma boa pista sobre o que esperar: assim como uma cebola tem várias camadas, o desafio consiste em uma mensagem que passou por **várias camadas de codificação empilhadas**, sendo necessário descascar uma de cada vez até chegar na flag.

## Análise Inicial

O enunciado do desafio é o seguinte:

> À primeira vista, parece apenas linguagem de máquina bruta, mas nossos analistas acreditam que o remetente empacotou a mensagem em várias camadas para burlar nossos filtros.
> Modelo de flag: FLAG{...}

Junto ao enunciado, é fornecido o seguinte conteúdo:

```
00110101 00110010 00100000 00110110 01100010 00100000 00110111 00111000 00100000 00110100 00110010 00100000 00110101 00110010 00100000 00110011 00110011 00100000 00110111 00110100 00100000 00110100 00110101 00100000 00110100 01100100 00100000 00110101 00111000 00100000 00110101 00111001 00100000 00110111 01100001 00100000 00110110 00110011 00100000 00110110 01100001 00100000 00110101 00110101 00100000 00110011 00110000 00100000 00110110 00110011 00100000 00110011 00110001 00100000 00110011 00111001 00100000 00110110 01100001 00100000 00110100 01100101 00100000 00110100 00110111 00100000 00110011 00110000 00100000 00110011 00110000 00100000 00110101 00110010 00100000 00110100 00110100 00100000 00110101 00110001 00100000 00110011 00110001 00100000 00110110 00110110 00100000 00110101 00110001 00100000 00110011 01100100 00100000 00110011 01100100
```

O próprio enunciado nos entrega a primeira pista: ele fala em "linguagem de máquina bruta", o que combinado com o padrão visual do conteúdo — grupos de 8 caracteres formados só por `0` e `1`, separados por espaço — deixa claro que essa primeira camada é **binário**.

## Interpretação

A palavra "camadas" no enunciado é o ponto-chave: em vez de uma única codificação, provavelmente vamos precisar decodificar o resultado de uma camada, obter uma nova string, e decodificar de novo, repetindo o processo até chegar em algo legível no formato `FLAG{...}`.

Como cada decodificação tende a produzir um resultado com uma "cara" característica (binário, hexadecimal, Base64 etc.), a estratégia foi ir decodificando camada por camada e observando o formato do resultado a cada passo, para saber qual a próxima codificação a reverter.

## Resolução

Para resolver, usamos a ferramenta online [CyberChef](https://cyberchef.org), empilhando as operações necessárias na "Recipe":

**1. Decodificando de Binário (From Binary)**

Colamos o conteúdo fornecido no Input e adicionamos a operação **"From Binary"**, com Delimiter "Space" e Byte Length "8" (já que os grupos têm 8 bits cada). O resultado é uma string composta só por caracteres de `0-9` e `a-f`, indicando que a próxima camada é **hexadecimal**.

**2. Decodificando de Hexadecimal (From Hex)**

Adicionamos a operação **"From Hex"** com Delimiter em "Auto" logo em seguida na Recipe. O resultado dessa camada é uma string terminando em `==`, um forte indício de que a camada seguinte é **Base64** (o `==` é o padding característico desse tipo de codificação).

**3. Decodificando de Base64 (From Base64)**

Por fim, adicionamos a operação **"From Base64"**, com o alfabeto padrão (`A-Za-z0-9+/=`) e a opção "Remove non-alphabet chars" marcada. Ao aplicar essa última camada, o Output já revela a flag em texto legível:

<img width="1426" height="834" alt="image" src="https://github.com/user-attachments/assets/8ae13628-87da-48a5-9077-a0fb100e7c69" />


```
FLAG{D1v3r54s_c4m4D45}
```

## Flag

```
FLAG{D1v3r54s_c4m4D45}
```

## Conclusão

Este desafio mostra como a **codificação em camadas** (ainda que não seja criptografia de verdade, já que codificação não usa uma chave secreta) pode ser usada para tentar disfarçar uma mensagem e dificultar sua leitura direta ou a detecção por filtros automáticos. A resolução reforça a importância de reconhecer o "formato visual" característico de cada tipo de codificação (binário, hexadecimal, Base64) para saber qual operação aplicar em cada etapa, descascando a mensagem camada por camada até chegar ao conteúdo original.
