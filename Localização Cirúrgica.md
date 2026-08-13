# Localização Cirúrgica (Escola de Cyber)

**Categoria:** OSINT / Geolocalização

## Introdução

Este desafio testa habilidades de **OSINT (Open Source Intelligence)** aplicadas à geolocalização de imagens. A proposta é simples de enunciar, mas exige precisão: dada uma fotografia de um monumento mundialmente conhecido, não basta identificar o local — é preciso descobrir a posição **exata** de onde a foto foi tirada, com precisão de poucos metros.

## Análise Inicial

O enunciado do desafio é o seguinte:

> Um monumento conhecido mundialmente pode ser encontrado em segundos. O verdadeiro desafio é descobrir exatamente onde o fotógrafo estava por meio de uma combinação de três palavras.
> Formato: FLAG{xxxx.xxxx.xxxx}

Junto ao enunciado, é fornecida uma imagem mostrando um grande mausoléu de mármore branco, com uma cúpula central em formato de bulbo, quatro minaretes simétricos nas laterais e um jardim formal com piscinas refletoras à frente.

{imagem: fotografia do monumento branco com cúpula central, quatro minaretes e piscinas refletoras em primeiro plano}

O formato da flag já entrega a segunda pista: `FLAG{xxxx.xxxx.xxxx}` é exatamente o padrão de um endereço do **what3words**, serviço que divide todo o planeta em quadrados de 3x3 metros, cada um identificado por uma combinação única de três palavras.

## Interpretação

Juntando as duas pistas do enunciado, o caminho fica claro:

1. **Identificar o monumento** — a arquitetura (cúpula em bulbo, mármore branco, quatro minaretes, jardins simétricos com piscina central) é um clássico exemplo de arquitetura mogol e é imediatamente reconhecível como o **Taj Mahal**, em Agra, na Índia.
2. **Encontrar o ponto de vista exato** — como a foto foi tirada de um ângulo específico dentro do complexo (não apenas "o Taj Mahal" de forma genérica), era necessário localizar o exato ponto do jardim onde o fotógrafo estava.
3. **Converter a coordenada em palavras** — usando o what3words, transformar as coordenadas geográficas exatas na combinação de três palavras pedida no formato da flag.

## Resolução

**1. Identificando o monumento**

A imagem fornecida foi facilmente reconhecida como o **Taj Mahal**. Para confirmar e localizar o ângulo exato, a estratégia foi abrir o **Google Maps**, buscar por "Taj Mahal" e ativar o modo **Street View**, navegando pelos pontos disponíveis dentro do jardim até encontrar um ângulo que reproduzisse fielmente a composição da imagem original: o mesmo caminho central, os mesmos ciprestes alinhados e a mesma piscina refletora emoldurando o monumento.

{imagem: resultado da busca "Taj Mahal" no Google Maps, com o painel de informações do local e o Street View aberto reproduzindo o mesmo ângulo da imagem do desafio}

**2. Extraindo as coordenadas exatas**

Ao posicionar o Street View no ângulo correspondente, a própria URL do Google Maps já contém as coordenadas exatas do ponto onde a câmera está localizada, no formato `@latitude,longitude,altura,heading,pitch,fov`:

```
27°10'23.4"N 78°02'30.6"E
```


Ou seja, o fotógrafo estava posicionado em **27°10'23.4"N 78°02'30.6"E**.

**3. Convertendo a coordenada em palavras (what3words)**

Com a coordenada em mãos, o próximo passo foi acessar o site **[what3words.com](https://what3words.com)** e buscar por essas coordenadas. O mapa do what3words divide a área em pequenos quadrados de 3x3 metros, e ao localizar o quadrado correspondente ao ponto exato de onde a foto foi tirada, o site exibe a combinação de três palavras daquele local — que é exatamente o valor a ser usado na flag.

<img width="1767" height="931" alt="image" src="https://github.com/user-attachments/assets/05d2044b-e541-4d70-8e8f-51d2e3a4a84b" />


O endereço retornado foi:

```
///commuted.anyway.stutter
```

Bastou então encaixar essas três palavras no formato indicado pelo enunciado (`FLAG{xxxx.xxxx.xxxx}`) para chegar à flag final:

```
FLAG{commuted.anyway.stutter}
```

## Flag

```
FLAG{commuted.anyway.stutter}
```

## Conclusão

Este desafio ilustra bem a lógica de resolução de tarefas de **OSINT/geolocalização**: primeiro, uma identificação ampla do local a partir de elementos visuais característicos (no caso, a arquitetura icônica do Taj Mahal); em seguida, um refinamento progressivo — usando ferramentas como Google Maps e Street View — até chegar às coordenadas exatas do ponto de captura da imagem; e, por fim, a tradução dessas coordenadas para o formato pedido pelo desafio usando o what3words. A combinação de reconhecimento visual com ferramentas de mapeamento é uma técnica recorrente em desafios desse estilo e reforça a importância de cruzar diferentes fontes (imagem, mapa e serviço de geocodificação) até obter uma resposta precisa — inclusive validando o quadrado exato marcado no what3words, já que um pequeno deslocamento na coordenada já é suficiente para gerar uma combinação de palavras completamente diferente.
