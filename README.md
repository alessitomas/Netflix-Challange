# Netflix-Challenge

Projeto da matéria de Álgebra Linear que possuía como finalidade criar um sistema preditor de nota de filmes por usuários com base em dados de avaliações de filmes realizadas pelos próprios usuários.


# Desafio

Pergunta milionária: eu vou gostar deste filme?

Nesse sentido, para responder essa pergunta precisamos prever qual nota um usuário atribuiria a um filme que ele ainda não assistiu. 
       
Utilizaremos a técnica SVD (Singular Value Decomposition) | (Decomposição em Valores Singulares) para prever o valor "real" de um elemento                estragado em uma matriz, ou seja, para prever a nota que um usuário deveria atribuir a um filme que ele ainda não assistiu.

# Processo

### Dataset
Dados passados de classificação que serão utilizados para auxiliar a previsão.

Descrição do conteúdo retirada do próprio site :

"conjunto de dados também contém arquivos contendo 26 milhões de classificações de 270.000 usuários para todos os 45.000 filmes. As classificações estão em uma escala de 1 a 5 e foram obtidas no site oficial do GroupLens",

[Link pra o dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset)

Com os dados criamos uma Matriz Original, definida por UserID X MovieID, onde os valores são os ratings (dados por cada user a cada filme)


$$
Matriz Original = \begin{bmatrix}
User_0 X Movie_0 & User_0 X Movie_1 & User_0 X Movie_2 & User_0 X Movie_3 & User_0 X Movie_4 & User_0 X Movie_5 & User_0 X Movie_n\\ 
User_1 X Movie_0 & User_1 X Movie_1 & User_1 X Movie_2 & User_1 X Movie_3 & User_1 X Movie_4 & User_1 X Movie_5 & User_1 X Movie_n\\
User_n X Movie_0 & User_n X Movie_1 & User_n X Movie_2 & User_n X Movie_3 & User_n X Movie_4 & User_n X Movie_5 & User_n X Movie_n
\end{bmatrix}
\hspace{0.5in}
$$



O primeiro passo é escolher aleatoriamente um dos elementos da matriz $B$, (desde que esse elemneto seja de fato um valor, não podendo ser nan)

Atribuir a ele um valor aleatório, gerando a matriz $B$. O que simula o caso em que um usuário ainda não assistiu a um filme e, sendo assim não atribuiu uma nota a ele, sendo assim nós estamos inserindo ruído na matrix orignal.

Decompondo a matriz $B$ 

Na decomposição SVD, usamos a formulação:


B = U @ Sigma @ V^T


onde:

* As colunas de U são os auto-vetores de $B^T B$,
* As colunas de V (e, portanto, as linhas de $V^T$) são auto-vetores de $B B^T$,
* Sigma é uma matriz onde $s_{i,i}$ é a raiz quadrada dos auto-valores de $B^T B$ ou de $B B^T$.

Matriz Sigma: Os valores singulares são números não negativos encontrados na diagonal principal da matriz Σ e representam a importância relativa das colunas e linhas da matriz original.

Com a 

O sistema então retorna o valor real que estava na matriz $A$ e o procedimento é repetido várias vezes, gerando um histograma dos erros cometidos. Isso permite avaliar a eficiência da técnica de SVD em prever os valores "reais" na matriz $A$. Quanto menor o erro médio, melhor a técnica de SVD é para prever as notas que um usuário daria a um filme que ele ainda não assistiu.

Considerando o dataset, formado por uma matrix de UserID X MovieID, onde os valores são os ratings (dados por cada user a cada filme). Podemos imaginar em uma forma de 



## Como o sistema funciona?

Primeiramente, o sistema recebe um dataframe com as avaliações dos usuários de determinados filmes que possui as seguintes colunas:

* `user_id` - ID do usuário
* `movie_id` - ID do filme
* `rating` - Nota do filme
* `timestamp` - Timestamp da avaliação (esta coluna não é utilizada, logo foi removida)

<br>


Em seguida, transformamos o dataframe recebido em uma matriz, onde cada linha representa um id do usuário e cada coluna representa um id do filme. Desta forma, cada elemento da matriz representa a avaliação do usuário para o filme. Caso o usuário não tenha avaliado o filme, o elemento da matriz é igual à NaN.

```python
matrix = ratings.pivot(index='userId', columns='movieId', values='rating')

# Matriz A que representa as avaliações dos usuários para os filmes
A = matrix.values
```

Saída:

```python
array([[nan, nan, nan, ..., nan, nan, nan],
       [nan, nan, nan, ..., nan, nan, nan],
       [nan, nan, nan, ..., nan, nan, nan],
       ...,
       [nan, nan, nan, ..., nan, nan, nan],
       [ 4., nan, nan, ..., nan, nan, nan],
       [ 5., nan, nan, ..., nan, nan, nan]])
```
<br>

Após esta etapa, criamos uma função chamada **indices_validos**, que recebe como parâmetro uma matriz e retorna uma lista com os índices dos elementos válidos da matriz. Um elemento é considerado válido se ele for diferente de NaN.

<br>

```python
def indices_validos(A):
    indices = []
    for i in range(A.shape[0]):
        for j in range(A.shape[1]):
            if not np.isnan(A[i][j]):
                indices.append((i, j))
    return indices
```
<br>

Além disso, criamos outra função chamada **processo**, que recebe como parâmetro a matriz original e a lista com os índices válidos desta matriz e realiza as seguintes etapas:

**1.** Seleciona um índice (linha, coluna) aleatório da lista de índices válidos;<br>

**2.** Armazena o valor original deste índice na variável ***valor_original***;<br>

**3.** Substitui por uma nota aleatória entre 0.5 e 5 diferente da nota original;<br>

**4.** Cria uma nova matriz ***B***, que é uma cópia da matriz original;<br>

**5.** Calcula o SVD (Singular Value Decomposition) da matriz ***B***, que é uma cópia da matriz original, gerando as matriz ***u***, ***s*** e ***vt***, que representam, respectivamente, os vetores esquerdos, os valores singulares e os vetores direitos da matriz ***B***;<br>

**6.** 








