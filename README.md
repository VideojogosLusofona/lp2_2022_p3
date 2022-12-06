<!--
Projeto 3 de Linguagens de Programação II 2022/23 (c) by Nuno Fachada

Projeto 3 de Linguagens de Programação II 2022/23 is licensed under a
Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.

You should have received a copy of the license along with this
work. If not, see <http://creativecommons.org/licenses/by-nc-sa/4.0/>.
-->

# Projeto 3 de Linguagens de Programação II 2022/23

## Introdução

Os grupos devem implementar, em Unity 2021.3 LTS, os estágios intermédios
de um [jogo 4X][4X], na continuação do [Projeto 1]. Em particular, devem ser
implementadas algumas unidades móveis com capacidade de agir no mundo de jogo.

O projetos devem ser desenvolvidos pelos mesmo grupos do [Projeto 1]. Quaisquer
alterações devem solicitadas ao [docente][Nuno Fachada] até **16 de dezembro**,
e só serão possíveis com forte justificação.

Até **16 de dezembro** é necessários que os grupos:

* Indiquem o URL do repositório **privado** do projeto no GitHub.
* Convidem o [docente][Nuno Fachada] para ter acesso a este mesmo repositório
  privado.

Os projetos serão automaticamente clonados às **23h00 de 8 de janeiro**, sendo
a entrega feita desta forma, sem intervenção dos alunos. Repositórios e/ou
projetos não funcionais nesta data não serão avaliados.

## Funcionamento da aplicação

A jogo base deve funcionar como indicado no enunciado do [Projeto 1], com as
seguintes diferenças:

* O código deve ser melhorado/corrigido tendo em conta o _feedback_ dado ao
  [Projeto 1].
* Devem ser removidos os botões "For the future", bem como o código e
  funcionalidades associadas.
* O jogo deve ter 4 tipos de unidades, que podem mover-se pelo mapa e colher e
  criar recursos, tal como indicado na tabela em baixo.
* Só pode existir uma unidade por _tile_ em cada momento.
* As unidades devem ser graficamente menores do que um _tile_, de modo a que
  seja possível clicar num _tile_ para obter a respetiva informação, mesmo que
  esteja lá uma unidade.
* Se o utilizador clicar numa unidade, essa unidade fica **selecionada**.
* Deve ser possível selecionar múltiplas unidades, selecionado uma área
  retangular do mundo de jogo com o rato (click+drag), **ou**, clicando numa
  unidade ao mesmo tempo que se pressiona a tecla CTRL (a unidade clicada é adicionada à seleção atual), **ou**, de ambas as formas.
  * Para cancelar a seleção atual, basta o utilizador selecionar outra
    unidade ou grupo de unidades, **ou**, clicar com o botão direito do rato em
    cima do mundo de jogo (seja em _tiles_ ou unidades).
  * A(s) unidade(s) selecionada(s) aceitam dois tipos de ordem:
    1. [Movimento](#movimento).
    2. [Colheita de recursos](#colheita-e-geração-de-recursos).
* Os turnos avançam de duas formas:
  1. O jogo avança um ou mais turnos quando é dada ordem de
     [movimento](#movimento).
  2. O jogo avança um turno quando é dada ordem de
     [colheita de recursos](#colheita-e-geração-de-recursos).
* Deve existir um painel lateral no UI, contendo:
  1. Um contador com a indicação do turno atual (os turnos avançam com o
     movimento das unidades, descrito mais em baixo).
  2. Um sub-painel que mostra os recursos existentes nos mundo de jogo (i.e.,
     para cada recurso possível, mostra a respetiva quantidade existente no mundo
     de jogo).
  3. Um sub-painel para mostrar informação sobre a unidade ou unidades
     selecionadas, em particular:
     - O painel deve estar vazio senão existir(em) unidade(s) selecionada(s).
     - Se estiver selecionada apenas uma unidade, deve ser mostrado o seu nome,
       bem como a lista de recursos (tipo e quantidade) que a unidade já colheu.
     - Se estiverem selecionadas duas ou mais unidades, devem ser mostrados a
       quantidade de unidades selecionadas, bem como a lista de recursos (tipo e
       quantidade) de todos os recursos capturados colhidos pelas unidades
       selecionadas.
     - No fundo deste sub-painel devem existir três botões:
       1. _Move_ - Após este botão ser pressionado, o ponteiro do rato muda,
          indicando que o utilizador pode selecionar um _tile_ de destino,
          clicando no mesmo (a ação de movimento pode ser cancelada clicando o
          botão direito do rato). Após seleção do _tile_ de destino, a(s)
          unidade(s) selecionada(s) move(m)-se de acordo com o descrito na secção
          [Movimento](#movimento).
       2. _Harvest_ - A(s) unidade(s) atualmente selecionada(s) realizam a
         colheita dos respetivos recursos (podendo também gerar recursos), de
         acordo com a secção [Colheita e geração de recursos](#colheita-e-geração-de-recursos).
      3. _Remove_ - Remove a(s) unidade(s) selecionada(s) do mundo de jogo.
  * Quatro botões em linha, para adicionar cada uma das quatro unidades no mapa,
    numa posição aleatória (inicialmente não devem existir unidades no mapa).

Devem ser implementadas quatro tipo de unidades, de acordo com a seguinte
tabela:

| Unidade     | Tipo de movimento        | Tipo de recursos que pode colher         |
|-------------|--------------------------|------------------------------------------|
| _Harvester_   | [Von Neumann]: ← ↑ → ↓   | Plants                                   |
| _Miner_       | [Moore]: ← ↖ ↑ ↗ → ↘ ↓ ↙ | _Metals_ (gera _Pollution_ ao colher _Metals_) |
| _Grim Reaper_ | [Moore]: ← ↖ ↑ ↗ → ↘ ↓ ↙ | _Plants_, _Animals_                          |
| _Butcher_     | [Von Neumann]: ← ↑ → ↓   | _Animals_                                  |

### Movimento

Quando é dada ordem de movimento, todas as unidades selecionadas tentam mover-se
para o _tile_ de destino da forma mais direta possível de acordo com o tipo de
movimento indicado na tabela anterior.

- Se uma unidade, ao decidir o seu próximo passo, for de encontro a outra, então
  a sua ação de movimento termina naquele momento.
- As unidades podem mover-se em todos os _tiles_, incluindo _water_.

### Colheita e geração de recursos

Quando é dada ordem de colheita de recursos, todas as unidades selecionadas
colhem recursos compatíveis, se os mesmos existirem no _tile_ a respetiva
unidade se encontra. Quando uma unidade colhe um recurso compatível, o mesmo
desaparece do _tile_ e é guardado na unidade. É de realçar que, ao contrário dos _tiles_, as unidades podem ter mais do que um recurso do mesmo tipo (por
exemplo, um _Harvester_ pode ter 3 plants, embora um _tile_ só possa ter um
recurso de cada tipo).

Se não existirem recursos compatíveis, i.e., recursos que a unidade possa
colher, então a unidade não faz nada.

Também é possível algumas unidades gerarem recursos. Em particular, a unidade
_Miner_, ao colher _Metals_ (e apenas ao colher _Metals_) gera _Pollution_ no
_tile_ onde se encontra. Caso o _tile_ já tenha _Pollution_, nada se altera, o
_tile_ simplesmente continua com _Pollution_.

Quando é dada ordem de colheita de recursos, é gasto um turno, mesmo que nenhuma
das unidades consiga colher recursos ou que várias o consigam fazer.

## Dicas e sugestões

### Organização do projeto e estrutura de classes

O projeto deve estar devidamente organizado, seguindo a fazendo uso de classes,
`struct`s e/ou enumerações, conforme seja mais apropriado. Cada tipo (i.e.,
classe, `struct` ou enumeração) deve ser colocado num ficheiro com o mesmo nome.
Por exemplo, uma classe chamada `Tile` deve ser colocada no ficheiro `Tile.cs`.
Por sua vez, a escolha da coleção ou coleções a usar também deve ser adequada
ao problema.

A estrutura de classes deve ser bem pensada e organizada de forma lógica,
fazendo uso de *design patterns* quando apropriado. Em particular, o projeto
deve seguir uma abordagem [MVC] (discutida em LP1, ver _slides_ do semestre
passado), e ser desenvolvido tendo em conta os princípios de programação
orientada a objetos, como é o caso, entre outros, dos princípios [SOLID].
Estes princípios devem ser balanceados com o princípio [KISS], crucial no
desenvolvimento de qualquer aplicação.

## Objetivos e critério de avaliação

Este projeto tem os seguintes objetivos:

* **O1** - Programa deve funcionar como especificado.
* **O2** - Projeto e código bem organizados, nomeadamente:
  * Estrutura de classes bem pensada (ver secção
    [Organização do projeto e estrutura de classes][orgclasses]).
  * Inexistência de código "morto", que não faz nada, como por exemplo
    variáveis, propriedades ou métodos nunca usados.
  * Projeto compila e executa sem erros e/ou *warnings*.
* **O3** - Código devidamente indentado, comentado e documentado. Documentação
  deve ser feita com [comentários de documentação XML][XML].
* **O4** - Repositório Git deve refletir boa utilização do mesmo, com
  *commits* de todos os elementos do grupo e mensagens de *commit* que sigam
  as melhores práticas para o efeito (como indicado
  [aqui](https://chris.beams.io/posts/git-commit/),
  [aqui](https://gist.github.com/robertpainsi/b632364184e70900af4ab688decf6f53),
  [aqui](https://github.com/erlang/otp/wiki/writing-good-commit-messages) e
  [aqui](https://stackoverflow.com/questions/2290016/git-commit-messages-50-72-formatting)).
  Quaisquer *assets* binários, tais como imagens, devem ser integrados
  no repositório em modo Git LFS (atenção que este último ponto é **muito
  importante**).
* **O5** - Relatório em formato [Markdown] (ficheiro `README.md`),
  organizado da seguinte forma:
  * Título do projeto.
  * Autoria:
    * Nome dos autores (primeiro e último) e respetivos números de aluno.
    * Informação de quem fez o quê no projeto. Esta informação é
      **obrigatória** e deve refletir os *commits* feitos no Git.
  * Legenda dos _tiles_, ou seja, que tipo de terreno e recursos representa cada
    _sprite_.
  * Legenda das unidades, ou seja, qual a unidade representada por cada
    _sprite_.
  * Arquitetura da solução:
    * Descrição da solução, com breve explicação de como o programa foi
      organizado, indicação dos _design patterns_ utilizados e porquê, bem como
      dos cuidados tidos ao nível dos princípios [SOLID].
    * Um diagrama UML de classes simples (i.e., sem indicação dos
      membros da classe) descrevendo a estrutura de classes (**obrigatório**).
  * Referências, incluindo trocas de ideias com colegas, código aberto
    reutilizado (e.g., do StackOverflow) e bibliotecas de terceiros
    utilizadas. Devem ser o mais detalhados possível.
  * **Nota:** o relatório deve ser simples e breve, com informação mínima e
    suficiente para que seja possível ter uma boa ideia do que foi feito.
    Atenção aos erros ortográficos e à correta formatação [Markdown], pois
    ambos serão tidos em conta na nota final.

O projeto tem um peso de 3.5 valores na nota final da disciplina e será avaliado
de forma qualitativa. Isto significa que todos os objetivos têm de ser
parcialmente ou totalmente cumpridos. A cada objetivo, O1 a O5, será atribuída
uma nota entre 0 e 1. A nota do projeto será dada pela seguinte fórmula:

*N = 3.5 x O1 x O2 x O3 x O4 x O5 x D*

Em que *D* corresponde à nota da discussão e percentagem equitativa de
realização do projeto, também entre 0 e 1. Isto significa que se os alunos
ignorarem completamente um dos objetivos, não tenham feito nada no projeto ou
não comparecerem na discussão, a nota final será zero.

## Entrega

O projeto é entregue de forma automática através do GitHub. Mais concretamente,
o repositório do projeto será automaticamente clonado às **23h00 de de 8 de
janeiro de 2023**. Certifiquem-se de que a aplicação está funcional e que todos
os requisitos foram cumpridos, caso contrário o projeto não será avaliado.

O repositório deve ter:

* Projeto Unity 2021.3 LTS funcional.
* Ficheiros `.gitignore` e `.gitattributes` adequados para projetos Unity.
* Ficheiro `README.md` contendo o relatório do projeto em formato [Markdown].
* Ficheiros de imagens, contendo o diagrama UML de classes e outras figuras
  que considerem úteis. Estes ficheiros devem ser incluídos no repositório em
  modo Git LFS (assim como todos os _assets_ binários do Unity).

Em nenhuma circunstância o repositório pode ter _builds_ ou outros ficheiros
temporários do Unity (que são automaticamente ignorados se usarem um
`.gitignore` apropriado).

## Honestidade académica

Nesta disciplina, espera-se que cada aluno siga os mais altos padrões de
honestidade académica. Isto significa que cada ideia que não seja do
aluno deve ser claramente indicada, com devida referência ao respetivo
autor. O não cumprimento desta regra constitui plágio.

O plágio inclui a utilização de ideias, código ou conjuntos de soluções
de outros alunos ou indivíduos, ou quaisquer outras fontes para além
dos textos de apoio à disciplina, sem dar o respetivo crédito a essas
fontes. Os alunos são encorajados a discutir os problemas com outros
alunos e devem mencionar essa discussão quando submetem os projetos.
Essa menção **não** influenciará a nota. Os alunos não deverão, no
entanto, copiar códigos, documentação e relatórios de outros alunos, ou dar os
seus próprios códigos, documentação e relatórios a outros em qualquer
circunstância. De facto, não devem sequer deixar códigos, documentação e
relatórios em computadores de uso partilhado.

Nesta disciplina, a desonestidade académica é considerada fraude, com
todas as consequências legais que daí advêm. Qualquer fraude terá como
consequência imediata a anulação dos projetos de todos os alunos envolvidos
(incluindo os que possibilitaram a ocorrência). Qualquer suspeita de
desonestidade académica será relatada aos órgãos superiores da escola
para possível instauração de um processo disciplinar. Este poderá
resultar em reprovação à disciplina, reprovação de ano ou mesmo suspensão
temporária ou definitiva da ULHT.

*Texto adaptado da disciplina de [Algoritmos e
Estruturas de Dados][aed] do [Instituto Superior Técnico][ist]*

## Referências

* \[1\] Whitaker, R. B. (2021). **The C# Player's Guide** (4th Edition).
  Starbound Software.
* \[2\] Albahari, J., & Johannsen, E. (2020). **C# 8.0 in a Nutshell**. O’Reilly
  Media.
* \[3\] Freeman, E., Robson, E., Bates, B., & Sierra, K. (2020). **Head First
  Design Patterns** (2nd edition). O'Reilly Media.
* \[4\] Dorsey, T. (2017). **Doing Visual Studio and .NET Code Documentation
  Right**. Visual Studio Magazine. Retrieved from
  <https://visualstudiomagazine.com/articles/2017/02/21/vs-dotnet-code-documentation-tools-roundup.aspx>.

## Licenças

* Este enunciado é disponibilizado através da licença [CC BY-NC-SA 4.0].

## Metadados

* Autor: [Nuno Fachada]
* Curso:  [Licenciatura em Videojogos][lamv]
* Instituição: [Universidade Lusófona de Humanidades e Tecnologias][ULHT]

[CC BY-NC-SA 4.0]:https://creativecommons.org/licenses/by-nc-sa/4.0/
[MIT]:http://opensource.org/licenses/MIT
[lamv]:https://www.ulusofona.pt/licenciatura/videojogos
[Nuno Fachada]:https://github.com/nunofachada
[ULHT]:https://www.ulusofona.pt/
[aed]:https://fenix.tecnico.ulisboa.pt/disciplinas/AED-2/2009-2010/2-semestre/honestidade-academica
[ist]:https://tecnico.ulisboa.pt/pt/
[Markdown]:https://guides.github.com/features/mastering-markdown/
[SOLID]:https://en.wikipedia.org/wiki/SOLID
[KISS]:https://en.wikipedia.org/wiki/KISS_principle
[XML]:https://docs.microsoft.com/dotnet/csharp/codedoc
[2º projeto de LP1 2018/19]:https://github.com/VideojogosLusofona/lp1_2018_p2_solucao
[orgclasses]:#organização-do-projeto-e-estrutura-de-classes
[objetivos]:#objetivos-e-critério-de-avaliação
[4X]:https://en.wikipedia.org/wiki/4X
[UnitySimpleFileBrowser]:https://github.com/yasirkula/UnitySimpleFileBrowser
[Formato dos ficheiros de mapa]:#formato-dos-ficheiros-de-mapa
[MVC]:https://www.geeksforgeeks.org/mvc-design-pattern/
[.NET SDK 6.0]:https://dotnet.microsoft.com/download/dotnet/6.0
[Projeto 1]:https://github.com/VideojogosLusofona/lp2_2022_p1
[Von Neumann]:https://en.wikipedia.org/wiki/Von_Neumann_neighborhood
[Moore]:https://en.wikipedia.org/wiki/Moore_neighborhood