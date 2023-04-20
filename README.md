# Netflix-Challenge

Projeto da matéria de Álgebra Linear que possuía como finalidade criar um sistema preditor de nota de filmes por usuários com base em dados de avaliações de filmes realizadas pelos próprios usuários.

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








