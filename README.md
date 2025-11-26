1. Código em C – “Desafio War Estruturado – Tema 1”

Arquivo sugerido: war_tema1.c
#include <stdio.h>
#include <string.h>

#define MAX_TERRITORIOS 5
#define TAM_NOME 30
#define TAM_COR  15

/* 
 * Estrutura básica de um território no jogo War.
 * Aqui já aplicamos o conceito de encapsulamento de dados:
 * tudo relacionado ao território fica agrupado dentro da struct.
 */
typedef struct {
    char nome[TAM_NOME];
    char cor[TAM_COR];
    int tropas;
} Territorio;

/* --------- ASSINATURAS DAS FUNÇÕES (protótipos) --------- */

void inicializarTerritorios(Territorio territórios[], int tamanho);
void cadastrarTerritorios(Territorio territórios[], int tamanho);
void exibirTerritorio(const Territorio *t);
void exibirTodosTerritorios(const Territorio territórios[], int tamanho);
void atualizarTropas(Territorio territórios[], int tamanho);
int  buscarTerritorioPorNome(const Territorio territórios[], int tamanho, const char *nome);
void limparEntrada();

/* ------------------------ MAIN --------------------------- */

int main(void) {
    Territorio territorios[MAX_TERRITORIOS];
    int opcao;

    inicializarTerritorios(territorios, MAX_TERRITORIOS);

    do {
        printf("\n=== DESAFIO WAR ESTRUTURADO - TEMA 1 ===\n");
        printf("1 - Cadastrar territorios\n");
        printf("2 - Listar territorios\n");
        printf("3 - Atualizar numero de tropas de um territorio\n");
        printf("0 - Sair\n");
        printf("Escolha uma opcao: ");
        if (scanf("%d", &opcao) != 1) {
            printf("Entrada invalida. Tente novamente.\n");
            limparEntrada();
            continue;
        }

        limparEntrada(); /* Remove \n e lixo do buffer */

        switch (opcao) {
            case 1:
                cadastrarTerritorios(territorios, MAX_TERRITORIOS);
                break;
            case 2:
                exibirTodosTerritorios(territorios, MAX_TERRITORIOS);
                break;
            case 3:
                atualizarTropas(territorios, MAX_TERRITORIOS);
                break;
            case 0:
                printf("Encerrando o programa...\n");
                break;
            default:
                printf("Opcao invalida. Tente novamente.\n");
        }

    } while (opcao != 0);

    return 0;
}

/* ---------------------- FUNÇÕES -------------------------- */

/*
 * inicializarTerritorios
 * 
 * Deixa todos os territorios com valores padrao.
 * Isso evita lixo de memoria e facilita debug.
 */
void inicializarTerritorios(Territorio territorios[], int tamanho) {
    int i;
    for (i = 0; i < tamanho; i++) {
        strcpy(territorios[i].nome, "N/A");
        strcpy(territorios[i].cor, "N/A");
        territorios[i].tropas = 0;
    }
}

/*
 * cadastrarTerritorios
 * 
 * Lê do teclado os dados de cada território.
 * Demonstra o uso de vetor de structs e manipulação
 * de strings com fgets.
 */
void cadastrarTerritorios(Territorio territorios[], int tamanho) {
    int i;

    printf("\n--- Cadastro de Territorios ---\n");

    for (i = 0; i < tamanho; i++) {
        printf("\nTerritorio %d de %d\n", i + 1, tamanho);

        printf("Nome do territorio: ");
        fgets(territorios[i].nome, TAM_NOME, stdin);
        territorios[i].nome[strcspn(territorios[i].nome, "\n")] = '\0';

        printf("Cor (ex: Vermelho, Azul, Verde): ");
        fgets(territorios[i].cor, TAM_COR, stdin);
        territorios[i].cor[strcspn(territorios[i].cor, "\n")] = '\0';

        printf("Quantidade de tropas: ");
        if (scanf("%d", &territorios[i].tropas) != 1) {
            printf("Valor invalido. Definindo tropas = 0.\n");
            territorios[i].tropas = 0;
        }

        limparEntrada();
    }

    printf("\nCadastro concluido!\n");
}

/*
 * exibirTerritorio
 * 
 * Recebe um ponteiro para Territorio (passagem por referencia)
 * e imprime os dados formatados. 
 */
void exibirTerritorio(const Territorio *t) {
    printf("Nome : %s\n", t->nome);
    printf("Cor  : %s\n", t->cor);
    printf("Tropas: %d\n", t->tropas);
}

/*
 * exibirTodosTerritorios
 *
 * Percorre o vetor e chama exibirTerritorio para cada elemento.
 */
void exibirTodosTerritorios(const Territorio territorios[], int tamanho) {
    int i;

    printf("\n--- Lista de Territorios ---\n");

    for (i = 0; i < tamanho; i++) {
        printf("\nTerritorio %d:\n", i + 1);
        exibirTerritorio(&territorios[i]);
    }
}

/*
 * buscarTerritorioPorNome
 *
 * Faz uma busca simples (linear) pelo nome do território.
 * Retorna o índice no vetor ou -1 se não encontrar.
 */
int buscarTerritorioPorNome(const Territorio territorios[], int tamanho, const char *nome) {
    int i;
    for (i = 0; i < tamanho; i++) {
        if (strcmp(territorios[i].nome, nome) == 0) {
            return i;
        }
    }
    return -1;
}

/*
 * atualizarTropas
 *
 * Permite ao usuario digitar o nome de um territorio
 * e atualizar a quantidade de tropas.
 */
void atualizarTropas(Territorio territorios[], int tamanho) {
    char nomeBusca[TAM_NOME];
    int novoValor;
    int indice;

    printf("\n--- Atualizar Tropas ---\n");
    printf("Digite o nome exato do territorio: ");
    fgets(nomeBusca, TAM_NOME, stdin);
    nomeBusca[strcspn(nomeBusca, "\n")] = '\0';

    indice = buscarTerritorioPorNome(territorios, tamanho, nomeBusca);

    if (indice == -1) {
        printf("Territorio '%s' nao encontrado.\n", nomeBusca);
        return;
    }

    printf("Territorio encontrado:\n");
    exibirTerritorio(&territorios[indice]);

    printf("\nDigite o novo numero de tropas: ");
    if (scanf("%d", &novoValor) != 1) {
        printf("Valor invalido. Operacao cancelada.\n");
        limparEntrada();
        return;
    }

    limparEntrada();
    territorios[indice].tropas = novoValor;

    printf("Tropas atualizadas com sucesso!\n");
}

/*
 * limparEntrada
 *
 * Remove o restante da linha atual do buffer do teclado.
 * Evita problemas com fgets depois de scanf.
 */
void limparEntrada() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF) {
        /* descarta caractere */
    }
}
2. Explicação profunda (para README ou relatório)

Você pode usar/adaptar esse texto no README.md do GitHub ou no seu trabalho escrito.

2.1. Visão geral do desafio

O Desafio War Estruturado – Tema 1 tem como objetivo aplicar conceitos fundamentais de estruturas de dados em C usando o contexto do jogo de tabuleiro War.

Neste primeiro tema (nível introdutório), o foco é:

Definir uma struct para representar um território do jogo;

Utilizar um vetor de structs para armazenar múltiplos territórios;

Organizar o código de forma estruturada em funções, separando responsabilidades;

Treinar a manipulação de strings e inteiros em C, além da interação básica com o usuário.

O jogo original War possui diversos territórios, cores e quantidades de tropas. Aqui, criamos uma versão simplificada, com 5 territórios, mas mantendo a ideia central de gerenciar as informações de cada um deles de forma organizada.

2.2. Estrutura de dados principal (struct Territorio)

O coração do programa é a estrutura:

typedef struct {
    char nome[TAM_NOME];
    char cor[TAM_COR];
    int tropas;
} Territorio;


Essa struct encapsula três informações essenciais de um território:

nome: armazena o nome do território (ex.: “Brasil”, “Peru”, “África do Sul”);

cor: representa a cor do jogador a que o território pertence (ex.: “Vermelho”, “Azul”);

tropas: quantidade de exércitos posicionados naquele território.

Usar uma struct traz vários benefícios:

Agrupa dados relacionados em uma única unidade lógica;

Facilita a passagem de territórios como parâmetro de funções;

Torna o código mais legível e mais fácil de manter.

Em seguida, usamos um vetor de Territorio:

Territorio territorios[MAX_TERRITORIOS];


Esse vetor representa uma “visão parcial” do mapa de War, guardando vários territórios ao mesmo tempo.

2.3. Organização estruturada em funções

Embora o código esteja em um único arquivo (war_tema1.c), a lógica foi organizada em várias funções, seguindo uma ideia de modularização:

Funções de inicialização e cadastro

inicializarTerritorios(...)

cadastrarTerritorios(...)

Funções de exibição

exibirTerritorio(...)

exibirTodosTerritorios(...)

Funções de busca e atualização

buscarTerritorioPorNome(...)

atualizarTropas(...)

Função utilitária

limparEntrada() – cuida do buffer de entrada do teclado.

Essa separação deixa o main() mais limpo e com uma função clara: coordenar o fluxo do programa, chamando funções específicas para cada ação.

2.4. Fluxo geral do programa (menu principal)

No main(), o programa segue o seguinte fluxo:

Cria o vetor de territórios:

Territorio territorios[MAX_TERRITORIOS];


Chama inicializarTerritorios para garantir que todos comecem com valores padrões:

inicializarTerritorios(territorios, MAX_TERRITORIOS);


Exibe um menu interativo em loop com as opções:

1 – Cadastrar territórios;

2 – Listar territórios;

3 – Atualizar número de tropas de um território;

0 – Sair.

Usa um switch para direcionar cada opção para a função correta.

Isso ilustra bem o conceito de programação estruturada: o main não se preocupa com detalhes de leitura ou exibição de cada campo, apenas chama as funções responsáveis.

2.5. Detalhamento das principais funções
2.5.1. inicializarTerritorios

Função:

void inicializarTerritorios(Territorio territorios[], int tamanho);


Percorre o vetor de territórios e define valores padrão:

nome = "N/A"

cor = "N/A"

tropas = 0

Evita trabalhar com lixo de memória (valores aleatórios) em variáveis não inicializadas.

Facilita debug: se aparecer “N/A”, você sabe que ainda não cadastrou aquele território.

2.5.2. cadastrarTerritorios

Função:

void cadastrarTerritorios(Territorio territorios[], int tamanho);


Para cada posição do vetor:

Lê nome com fgets;

Lê cor com fgets;

Lê tropas com scanf.

Remove o '\n' que o fgets coloca no final da string usando:

territorios[i].nome[strcspn(territorios[i].nome, "\n")] = '\0';


Mostra o uso prático de strings em C, que são vetores de char terminados em '\0'.

2.5.3. exibirTerritorio e exibirTodosTerritorios

exibirTerritorio recebe um ponteiro para Territorio:

void exibirTerritorio(const Territorio *t);


Essa assinatura demonstra passagem por referência (via ponteiro), tema muito importante em C.

Usa o operador -> para acessar os campos da struct.

exibirTodosTerritorios apenas percorre o vetor e chama exibirTerritorio para cada elemento:

void exibirTodosTerritorios(const Territorio territorios[], int tamanho);


Esse padrão evita repetição de código: se futuramente o formato de exibição mudar, basta alterar exibirTerritorio.

2.5.4. buscarTerritorioPorNome e atualizarTropas

buscarTerritorioPorNome implementa uma busca linear simples:

int buscarTerritorioPorNome(const Territorio territorios[], int tamanho, const char *nome);


Compara o nome passado com o territorios[i].nome usando strcmp.

Retorna o índice do vetor ou -1 se não encontrar.

atualizarTropas usa essa função para localizar o território e, em seguida, atualizar o campo tropas:

void atualizarTropas(Territorio territorios[], int tamanho);


Se o território é encontrado, mostra os dados atuais;

Lê o novo valor de tropas;

Atualiza o struct correspondente.

Esse padrão reforça a ideia de reutilizar funções para evitar duplicação de lógica.

2.5.5. limparEntrada
void limparEntrada() {
    int c;
    while ((c = getchar()) != '\n' && c != EOF) {
        /* descarta caractere */
    }
}


Serve para limpar o buffer de entrada após um scanf.

Sem essa função, o '\n' da tecla Enter poderia atrapalhar a próxima chamada de fgets, fazendo ela ler uma string vazia.

É uma função simples, mas crucial para programas interativos em C.

2.6. Possíveis evoluções (para outros temas)

Esse código resolve bem o Tema 1, mas já está preparado para crescer:

Adicionar mais campos na struct, como:

Continente;

ID do jogador;

Status (conquistado, em disputa etc.).

Migrar para uma organização com módulos separados:

Arquivo war.h com a definição da struct e protótipos;

Arquivo war.c com a implementação das funções;

Arquivo main.c apenas com a função main.

Implementar alocação dinâmica (malloc) para permitir uma quantidade variável de territórios;

Criar operações mais “War-like”, como:

Distribuir tropas automaticamente;

Simular um ataque entre dois territórios;

Controlar missões e objetivos.
