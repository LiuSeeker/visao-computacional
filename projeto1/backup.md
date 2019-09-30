## Introdução
A Protanopia e a Deoutoranopia são tipos específicos de daltonismo. O indivíduo com protanopia tem dificuldades de percepção de tons de cores vermelhos; e o indivíduo com deutoranopia tem dificuldades de percepção de tons de cores verdes. Tal dificuldade é causada por um mal funcionamento das células com a capacidade de reconhecimento de cores, chamadas cones.

###### Esse ou...
A carência de cones funcinais faz com que os cones L não gere estímulos suficientes quando ondas de comprimento longos, associados à cor vermelha, atingem os olhos; e, de igual modo, os cones M com ondas de comprimento médios, associados à cor verde.
###### esse
No caso da protanopia, os cones L, responsáveis pela percepção de ondas de comprimento longos, associados à cor vermelha, não geram estímulos suficientes para intepretar a onda recebida; de mesmo modo, no caso da deutoranopia, os cones M, responsáveis pela percepção de ondas de comprimento médios, associados à cor verde.

## Objetivo
O objetivo deste projeto é simular a protanopia e deuteranopia. A partir do artigo "Digital Video Colourmaps for Checking the Legibility of Displays by Dichromats", de Françoise Viénot, Hans Brettel e John D. Mollon, publicado em Janeiro de 1999, os autores discutem a relação entre o modelo de cor RGB, comumente associado à imagens reproduzidas em dispositivos eletrônicos, e o modelo LMS, baseado nos 3 comprimentos de onda que o olho humano é capaz de interpretar como cores, graças às células cones. Com a relação entre os dois modelos fundamentada, os autores associam a questão da protanopia e da deuteranopia com a relação entre o modelo LMS e RGB, assim obtendo um caminho para a simulação desejada neste projeto.

## Metodologia
O trabalho se baseará na transformação da imagem abaixo, uma vez para simulação de protanopia e outra para deuteranopia

Os passos para a transformação de imagens para protanopia e deuteranopia de acordo com o artigo são:

**1** - Converter os valores de 8 bits do conversor digital-analógico (DAC) para valores RGB, invertendo os valores gamma do monitor (gamut compression);

\begin{align}
R & = (I/255)^{2.2} \\
G & = (J/255)^{2.2} \\
B & = (K/255)^{2.2}
\end{align}


**2** - Redução do domínio das cores para a compatibilidade do monitor para correção da colorimetria do monitor.

Para protanopia:
\begin{align}
R_2 & = 0.992052 \cdot R + 0.003974  \\
G_2 & = 0.992052 \cdot G + 0.003974  \\
B_2 & = 0.992052 \cdot B + 0.003974
\end{align}

Para deuteranopia:
\begin{align}
R_2 & = 0.957237 \cdot R + 0.0213814  \\
G_2 & = 0.957237 \cdot G + 0.0213814  \\
B_2 & = 0.957237 \cdot B + 0.0213814
\end{align}

**3** - Baseado na recomendação ITU-R BT709 e nas transformações colorimétricas de Judd-Vos, e Smith e Pokorny, fazer a transformação do dominio RGB obtidos no passo 2 para o dominio LMS

\begin{align}
\begin{bmatrix}
L \\ M \\ S
\end{bmatrix} =
\begin{bmatrix}
17.8824 & 43.5161 & 4.11935 \\ 3.45565 & 27.1554 & 3.86714 \\ 0.0299566 & 0.184309 & 1.46709
\end{bmatrix}
\times 
\begin{bmatrix}
R_2 \\ G_2 \\ B_2
\end{bmatrix}
\end{align}

**4** - Redução do dominio LMS para o dominio das cores LMS com protanopia e deuteranopia

Para protanopia:
\begin{align}
\begin{bmatrix}
L_p \\ M_p \\ S_p
\end{bmatrix} =
\begin{bmatrix}
0 & 2.02344 & -2.52581 \\ 0 & 1 & 0 \\ 0 & 0 & 1
\end{bmatrix}
\times 
\begin{bmatrix}
L \\ M \\ S
\end{bmatrix}
\end{align}

Para deuteranopia:
\begin{align}
\begin{bmatrix}
L_d \\ M_d \\ S_d
\end{bmatrix} =
\begin{bmatrix}
1 & 0 & 0 \\ 0.494207 & 0 & 1.24827 \\ 0 & 0 & 1
\end{bmatrix}
\times 
\begin{bmatrix}
L \\ M \\ S
\end{bmatrix}
\end{align}

**5** - Com os valores do domínio LMS alteradas para simular a protanopia e deuteranopia, reverter para o domínio RGB

\begin{align}
\begin{bmatrix}
R_{mod} \\ G_{mod} \\ B_{mod}
\end{bmatrix} = \begin{bmatrix}
0.080944 &  -0.130504 & 0.116721 \\ - 0.0102485 & 0.0540194 & -0.113615 \\ -0.000365294 & -0.00412163 & 0.693513
\end{bmatrix}
\times 
\begin{bmatrix}
L_{mod} \\ M_{mod} \\ S_{mod}
\end{bmatrix}
\end{align}

## Resultados
Para a transformação para protanopia, os resultados mostram que a imagem simulando protanopia se aproxima do esperado, porém com pode-se notar que há carência de tons avermelhados. Apesar da protanopia ser a deficiência de cones L, prejudicando a percepção de tons avermelhados, os cones M ainda sim conseguem captar um pouco das ondas de comprimento correspondente ao laranja.

Para a transformação para deuteranopia, os resultados foram ligeiramente diferente ao esperado. A imagem simulando a deuteranopia mostra uma grande similaridade  grande com a imagem simulando a protanopia, caracterizada pela presença de tons amarelos, porém não mostra tons amarronzados esperados.

As diferenças analisadas podem ter ocorrido pelo fato dos autores terem considerado uma paleta de 216 cores, de 8-bits. Atualmente, é comum os computadores terem uma paleta de cores de 18-bits ou mais.

Outro possível motivo para as diferenças ocorridas é a utilização da recomendação ITU-R BT709, padrão de utilização de cores baseadas no diagrama cromático, que é bem ultrapassado em relação à recomendação atual (ITU-R BT2100)

## Conclusão

A simulação de protanopia se mostrou satisfatória apesar de não ser totalmente perfeita. Já a reprodução de deuteranopia ficou significamente distinto do esperado, mostrando que a modelo de simulação de deuteranopia proposto pelo artigo não é um modelo adequado.
Para estudos sobre o artigo apresentado, as considerações de paletas de cores de computadores mais recentes e o uso de recomendações ITU-R mais atualizadas são relevantes para possivelmente atingir resultados de simulação de protanopia e deuteranopia mais próximos da realidade.


###### http://gshow.globo.com/Bastidores/noticia/2016/05/drfernandoresponde-tabela-mostra-os-tipos-de-daltonismo.html
imagem.jpg

###### https://aminoapps.com/c/furry-pt/page/blog/daltonismo-discromatopsia/vzxb_3rCnuLJpDMKr07law8z02bJZVpEE
imagem2.jpg

###### https://clubjimmy.com/voir-le-monde-comme-les-daltoniens/
teste 2.jpg

###### https://mycourses.aalto.fi/pluginfile.php/630975/mod_resource/content/1/Luento%208%202018.pdf
teste.jpg

###### https://omaharentalads.com/explore/marks-clipart-high-grade/
arco_iris.png

