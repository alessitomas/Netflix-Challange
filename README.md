# Netflix-Challenge

Projeto da matéria de Álgebra Linear que possuía como finalidade criar um sistema preditor de nota de filmes por usuários com base em dados de avaliações de filmes realizadas pelos próprios usuários.


# Desafio

Pergunta milionária: ***eu vou gostar deste filme?***

Nesse sentido, para responder essa pergunta precisamos prever qual nota um usuário atribuiria a um filme que ele ainda não assistiu. 
       
Utilizaremos a técnica **SVD** (Singular Value Decomposition) | (Decomposição em Valores Singulares) para prever o valor "real" de um elemento estragado em uma matriz, ou seja, para prever a nota que um usuário deveria atribuir a um filme que ele ainda não assistiu.

# Processo

### Dados
Dados passados de classificação que serão utilizados para auxiliar a previsão.

[Link pra o dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset)


<br>

Com os dados criamos uma Matriz Original, definida por UserID X MovieID, onde os valores são os ratings (dados por cada user a cada filme)

<br>

$$
Matriz Original = \begin{bmatrix}
User_0 X Movie_0 & User_0 X Movie_1 & User_0 X Movie_2 & User_0 X Movie_3 & User_0 X Movie_4 & User_0 X Movie_5 & User_0 X Movie_n\\ 
User_1 X Movie_0 & User_1 X Movie_1 & User_1 X Movie_2 & User_1 X Movie_3 & User_1 X Movie_4 & User_1 X Movie_5 & User_1 X Movie_n\\
User_n X Movie_0 & User_n X Movie_1 & User_n X Movie_2 & User_n X Movie_3 & User_n X Movie_4 & User_n X Movie_5 & User_n X Movie_n
\end{bmatrix}
\hspace{0.5in}
$$

<br>

O primeiro passo é escolher aleatoriamente um dos elementos da matriz $B$, (desde que esse elemneto seja de fato um valor, não podendo ser nan)

Atribuir a ele um valor aleatório, gerando a matriz $B$. O que simula o caso em que um usuário ainda não assistiu a um filme e, sendo assim não atribuiu uma nota a ele, sendo assim nós estamos inserindo ruído na matriz orignal.

<br>

$$
B = \begin{bmatrix}
Ruído & User_0 X Movie_1 & User_0 X Movie_2 & User_0 X Movie_3 & User_0 X Movie_4 & User_0 X Movie_5 & User_0 X Movie_n\\ 
User_1 X Movie_0 & User_1 X Movie_1 & User_1 X Movie_2 & User_1 X Movie_3 & User_1 X Movie_4 & User_1 X Movie_5 & User_1 X Movie_n\\
User_n X Movie_0 & User_n X Movie_1 & User_n X Movie_2 & User_n X Movie_3 & User_n X Movie_4 & User_n X Movie_5 & User_n X Movie_n
\end{bmatrix}
\hspace{0.5in}
$$

<br>

### Decompondo a matriz $B$ 

Na decomposição SVD, usamos a formulação:

<br>

$$
B = U @ Sigma @ V^T
$$

<br>

onde:

* As colunas de U são os auto-vetores de $B^T B$,
* As colunas de V (e, portanto, as linhas de $V^T$) são auto-vetores de $B B^T$,
* Sigma é uma matriz onde $s_{i,i}$ é a raiz quadrada dos auto-valores de $B^T B$ ou de $B B^T$.

<br>

**Matriz Sigma**: os auto valores são números não negativos encontrados na diagonal principal da matriz Σ e representam a importância relativa das colunas e linhas da matriz original.

Com a inserção de ruído os auto valores menores são os mais afetados, e por isso iremos buscar reduzir essa diferança para que assim possamos voltar o mais próximo da matriz original, partindo de uma matriz com ruído (no nosso caso de um ponto onde não temos rating).

Para realizar a redução de dimensionalidade, a decomposição SVD é aplicada na matriz original, e os auto valores maior são selecionados e os demais são descartados. As colunas correspondentes aos valores singulares maiores são selecionadas e formam uma matriz menor new_Matriz, que é uma aproximação da Matriz Original, partindo de uma matriz com ruído B .

A partir da new_Matriz podemos acessar um valor, e comparar a diferença dele com o valor real na Matriz Original.

Executando o processo diversas vezes é possível gerar um histograma dos erros cometidos. O que permite avaliar a eficiência do SVD em prever os valores "reais" na Matriz Original. Quanto menor o erro médio, melhor a técnica de SVD é para prever as notas que um usuário daria a um filme que ele ainda não assistiu










