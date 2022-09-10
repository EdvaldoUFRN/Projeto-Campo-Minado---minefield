#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <unistd.h>
// USAMOS AQUI UMA ESTRUTURA typedef
typedef struct {
  int minas;
  int vizinhos;
  int controle;
  int automatico;
} minado;
// Iremos utilizar uma variável global campo, matriz.
minado Campo[10][20];
// variável global para linha e coluna
int linha = 10;
int coluna = 20;
// Função para Verificar se é valida a coordenada
int coordenada_valida(int l, int c) {
  if (l < 0 || l > 9 || c < 0 || c > 19) {
    return 0;
  } else {
    return 1;
  }
}
// Função para zerar todos os valores.
void zerar() {
  for (int i = 0; i < 10; i++) {
    for (int j = 0; j < 20; j++) {
      Campo[i][j].controle = 0;
      Campo[i][j].minas = 0;
      Campo[i][j].vizinhos = 0;
      Campo[i][j].automatico = 0;
    }
  }
}
// sorteio de bombas de maneira aleatória
void bombas() {
  srand(time(NULL));
  for (int i = 0; i < 40; i++) {
    linha = rand() % 10;
    coluna = rand() % 20;
    if (Campo[linha][coluna].minas == 0) {
      Campo[linha][coluna].minas = 1;
    } else {
      i--;
    }
  }
}
// analisar as bombas próximas
int bombavizinha(int linha_coordenada, int coluna_coordenada) {
  int bomba = 0;
  if (coordenada_valida(linha_coordenada - 1, coluna_coordenada + 1) == 1 &&
      Campo[linha_coordenada - 1][coluna_coordenada + 1].minas == 1) {
    bomba++;
  }

  if (coordenada_valida(linha_coordenada, coluna_coordenada + 1) == 1 &&
      Campo[linha_coordenada][coluna_coordenada + 1].minas == 1) {
    bomba++;
  }
  if (coordenada_valida(linha_coordenada - 1, coluna_coordenada) == 1 &&
      Campo[linha_coordenada - 1][coluna_coordenada].minas == 1) {
    bomba++;
  }
  if (coordenada_valida(linha_coordenada + 1, coluna_coordenada) == 1 &&
      Campo[linha_coordenada + 1][coluna_coordenada].minas == 1) {
    bomba++;
  }

  if (coordenada_valida(linha_coordenada + 1, coluna_coordenada - 1) == 1 &&
      Campo[linha_coordenada + 1][coluna_coordenada - 1].minas == 1) {
    bomba++;
  }

  if (coordenada_valida(linha_coordenada + 1, coluna_coordenada + 1) == 1 &&
      Campo[linha_coordenada + 1][coluna_coordenada + 1].minas == 1) {
    bomba++;
  }

  if (coordenada_valida(linha_coordenada - 1, coluna_coordenada - 1) == 1 &&
      Campo[linha_coordenada - 1][coluna_coordenada - 1].minas == 1) {
    bomba++;
  }
  if (coordenada_valida(linha_coordenada, coluna_coordenada - 1) == 1 &&
      Campo[linha_coordenada][coluna_coordenada - 1].minas == 1) {
    bomba++;
  }

  return bomba;
}
// ANALISAR OS VIZINHOS E COLOCAR AS BOMBAS QUE ESTÃO PRÓXIMAS
void vizin_analisado() {
  for (int i = 0; i < 10; i++) {
    for (int j = 0; j < 20; j++) {
      if (coordenada_valida(i, j) == 1) {
        Campo[i][j].vizinhos = bombavizinha(i, j);
      }
    }
  }
}
// Função  para abrir os espaços quando um for zero abrirá os outros espaços
void abrir(int linha, int coluna) {
  if (coordenada_valida(linha, coluna) == 1 &&
      Campo[linha][coluna].controle == 0) {
    Campo[linha][coluna].controle = 1;
    if (Campo[linha][coluna].vizinhos == 0 && Campo[linha][coluna].minas == 0) {
      abrir(linha - 1, coluna + 1);
      abrir(linha, coluna + 1);
      abrir(linha - 1, coluna);
      abrir(linha + 1, coluna);
      abrir(linha + 1, coluna - 1);
      abrir(linha + 1, coluna + 1);
      abrir(linha - 1, coluna - 1);
      abrir(linha, coluna - 1);
    }
  }
}

// Declarei uma função
void jogo(int variacao, int contagem);
void tentativa(int *contador, int contagem);
void interface();
void menu_do_jogo();
void jogo_automatico();
void printa_texto();

// DESSE MODO NÃO FOI PRECISO DECLARAR AS FUNÇÕES.
// BASTOU SEGUIR ESSA ORDEM.

// CAMPO FEITO
void campo() {
  printf("\n\t\t---------------------------CAMPO "
         "MINADO---------------------------\n\n\n");
  printf("\t   ");
  for (int i = 0; i < 20; i++) {
    if (i < 10) {
      printf(" %d ", i);
    }
    if (i == 10) {
      printf(" %d ", i);
    }
    if (i > 10) {
      printf("%d ", i);
    }
  }
  printf("\n");
  printf(
      "\t   -------------------------------------------------------------\n");
  for (int j = 0; j < 10; j++) {
    printf("    %d --> ", j);
    printf(" |");
    for (int k = 0; k < 20; k++) {
      if (Campo[j][k].controle == 1) {
        if (Campo[j][k].minas == 1) {
          printf("*");
        } else {
          printf("%d", Campo[j][k].vizinhos);
        }
      } else {
        printf(" ");
      }
      printf(" |");
    }
    printf("\n");
    printf(
        "\t   -------------------------------------------------------------\n");
  }
  // fim
  printf("\n");
}
// isso daqui tem que aparecer apenas uma vez.
void sub_interface() {
  printf("\n");
  printf("\t\t\t\t1-Inicar Jogo.\n");
  printf("\t\t\t\t2-Instruções.\n");
  printf("\t\t\t\t3-Sair.\n\n");
}
// FUNÇÃO GANHAR VAI BASICAMENTE VERIFICAR TODOS OS CAMPOS, PRA VER SE ESTÃO
// FECHADOS E SE NÃO CONTEM UMA MINA.
int ganhar() {
  int contagem = 0;
  for (int i = 0; i < 10; i++) {
    for (int j = 0; j < 20; j++) {
      if (Campo[i][j].controle == 0 && Campo[i][j].minas == 0) {
        contagem++;
      }
    }
  }
  return contagem;
}
// TRANSFORMEI EM INTERFACE
void interface() {
  int opcao;
  int volta;
  printf("\t\t\t\t\tCAMPO MINADO\n");
  sub_interface();
  printf("\t\t\tEscolha um número para continuar: ");
  // printf("\t\t\t\tEscolha uma das 4 opções acima: ");
  scanf("%d", &opcao);
  if (opcao < 1 || opcao > 3) {
    system("clear");
    do {
      printf("\t\t\t\t\tCAMPO MINADO\n");
      sub_interface();
      printf("\t\t\tError!\n\t\t\tDigite somente números de 1 a 4.\n");
      printf("\t\t\tEscolha um número para continuar: ");
      scanf("%d", &opcao);
      system("clear");
    } while (opcao < 1 || opcao > 3);
  }
  // UTILIZAR SWITCH CASE.
  switch (opcao) {
  case 1:
    // ABRE MENU
    system("clear");
    printf("\n\n\n\n\t\t\t\t\tCarregando");
    for (int i = 0; i < 5; i++) {
      system("sleep 01");
      printf(".");
      fflush(stdout);
    }
    system("clear");
    menu_do_jogo();
    break;
  case 2:
    // ABRE INFORMAÇÕES
    system("clear");
    printf("\n\n\n\n\t\t\t\t\tCarregando");
    for (int i = 0; i < 2; i++) {
      system("sleep 01");
      printf(".");
      fflush(stdout);
    }
    system("clear");
    printf("\n");
    printf("\t\t\t\t\t\t#INSTRUÇÕES\n\n");
    printf("\t\t1.Você irá informar a linha e a coluna da coordenada desejada "
           "pra abrir.\n");
    printf("\t\t2.Se você descobrir uma mina, o jogo acaba.\n");
    printf("\t\t3.Se descobrir um quadrado vazio, o jogo continua.\n");
    printf(
        "\t\t4.Se aparecer um número, ele informará quantas minas estão "
        "escondidas nos oito quadrados que o cercam. Você usa essa informação "
        "para deduzir em que quadrados próximos é seguro clicar.\n");
    printf("\n\n\n\n\n\n\n");
    printf("Para voltar digite o número 1: ");
    do {
      scanf("%d", &volta);
    } while (volta != 1);
    if (volta == 1) {
      system("clear");
      printf("\n\n\n\n\t\t\t\t\tCarregando");
      for (int i = 0; i < 1; i++) {
        system("sleep 01");
        printf(".");
        fflush(stdout);
      }
      system("clear");
      interface();
    }
    break;
  case 3:
    exit(0);
    break;
  }
}

// FUNÇÃO MENU DO JOGO
void menu_do_jogo() {
  int escolha;
  int dentro;
  campo();
  printf("\t\t\t\t---Menu principal---\n\n");
  printf("\t\t\t1.Modo manual iniciando com zero.\n");
  printf("\t\t\t2.Modo manual iniciando com um número.\n");
  printf("\t\t\t3.Modo Automático.\n");
  printf("\t\t\t4.Informações.\n");
  printf("\t\t\tPara continuar digita o número desejado: ");
  scanf("%d", &escolha);
  // MODO DE JOGO ONDE A PRIMEIRA JOGADA CAIRÁ EM UMA CASA 0
  if (escolha == 1) {
    system("clear");
    jogo(0, 0);
  }
  // MODO DE JOGO ONDE SUA PRIMEIRA JOGADA CAIRÁ EM UM NÚMERO
  if (escolha == 2) {
    system("clear");
    jogo(2, 0);
  }
  // ESCOLHA PARA MODO DE JOGO AUTOMÁTICO
  if (escolha == 3) {
    system("clear");
    jogo_automatico();
  }
  // ABRE A TELA DE INFORMAÇÕES
  if (escolha == 4) {
    do {
      system("clear");
      printf("\t\t\tINFORMAÇÕES\n\n\n");
      printf(
          "\t\t\tModo manual: Você vai ter que tomar as próprias decisões.\n");
      printf("\t\t\tModo Automático: O computador irá jogar sozinho.\n");
      printf("\t\t\tA sua primeira jogada nunca será uma bomba caso escolha o "
             "modo manual iniciando com zero.");
      printf("\n\n\t\t\tPara voltar digita 1: ");
      scanf("%d", &dentro);
      if (dentro == 1) {
        system("clear");
        menu_do_jogo();
      }

    } while (dentro != 1);
  }
}
// FUNÇÃO TENTATIVAS - PASSAGEM POR REFERÊNCIA E USO DE ALOCAÇÃO DINÂMICA
void tentativa(int *contador, int contagem) {
  *contador = *contador * contagem;
}

// FUNÇÃO QUE VAI DAR INÍCIO AO JOGO.
void jogo(int variacao, int contagem) {
  zerar();
  bombas();
  vizin_analisado();
  int *contador;
  contador = malloc(sizeof(int) * 1);
  *contador = 1;
  time_t begin = time(NULL);
  int lin, colun, jogar, selecao;
  int control = 0;
  do {
    system("clear");
    campo();
    printf("\t\t\tSELECIONE:\n");
    printf("\t\t\tDigite 1 para selecionar a célula.\n");
    printf("\t\t\tDigite 2 para pedir ajuda.\n");
    printf("\t\t\tDigite 3 para checar o tempo gasto.\n");
    printf("\t\t\tFaça sua escolha: ");
    scanf("%d", &selecao);
    if (selecao == 1) {
      do {
        printf("Digite as coordenadas [linha, coluna]:\n");
        scanf("%d %d", &lin, &colun);
        if (coordenada_valida(lin, colun) != 1) {
          printf("Coordenada inválida!\n\n");
        }
        if (Campo[lin][colun].controle == 1) {
          printf("Coordenada já aberta! Selecione outra coordenada.\n\n");
        }
        if (control == 0) {
          if (variacao == 0) {
            do {
              zerar();
              bombas();
              vizin_analisado();
            } while (Campo[lin][colun].vizinhos != 0 ||
                     Campo[lin][colun].minas == 1);
          }
        }
        if (control == 0) {
          // BOTÃO AJUDA
          if (variacao == 2) {
            do {
              zerar();
              bombas();
              vizin_analisado();
            } while (Campo[lin][colun].vizinhos == 0 ||
                     Campo[lin][colun].minas == 1);
          }
        }
        control = 1;
      } while (coordenada_valida(lin, colun) == 0 ||
               Campo[lin][colun].controle == 1);
      abrir(lin, colun);
    }
    if (selecao == 2) {
      do {
        lin = rand() % 10;
        colun = rand() % 20;
      } while (coordenada_valida(lin, colun) == 0 ||
               Campo[lin][colun].minas == 1);
      printf("\n\t Que tal a coordenada %d %d?", lin, colun);
    }

    if (selecao == 3) {
      time_t end = time(NULL);
      printf("\n\n\n\tO tempo gasto foi de %d segundos.", (end - begin));
    }
    fflush(stdin);
  } while (ganhar() != 0 && Campo[lin][colun].minas == 0);
  if (Campo[lin][colun].minas == 1) {
    system("clear");
    campo();
    printa_texto("derrota.txt");
    printf("\n\tBomba encontrada!!!\n\tVocê perdeu!!!\n\n\n");
    time_t end = time(NULL);
    tentativa(contador, contagem);
    printf("\tEssa foi sua tentativa número %d\n", *contador);
    printf("\tO tempo gasto foi de %d segundos.\n", (end - begin));
    for (int i = 0; i < 6; i++) {
      system("sleep 01");
      if (i == 0) {
        printf("\t*Para jogar novamente digita 1.\n");
      }
      if (i == 1) {
        printf("\t*Para voltar pro menu principal digita 2.\n");
      }
      if (i == 3) {
        printf("\t*Para voltar para o menu inicial digite 3. \n");
      }
      if (i == 4) {
        printf(
            "\t*Para sair do jogo digita qualquer diferente de 1, 2 ou 3.\n\n");
      }
      if (i == 5) {
        printf("\t Digita aqui: ");
        scanf("%d", &jogar);
      }
      fflush(stdout);
    }
    if (jogar == 1) {
      system("clear");
      jogo(variacao, contagem + 1);
    }
    if (jogar == 2) {
      system("clear");
      menu_do_jogo();
    }
    if (jogar == 3) {
      system("clear");
      interface();
    } else {
      exit(0);
    }
  } else {
    system("clear");
    campo();
    printa_texto("vitoria.txt");
    printf("\n\tPARABÉNS!!!\n\tVocê ganhou!!!\n\n\n");
    time_t end = time(NULL);
    tentativa(contador, contagem);
    printf("\tEssa foi sua tentativa número %d\n", *contador);
    printf("\tO tempo gasto foi de %d segundos.\n", (end - begin));
    for (int i = 0; i < 5; i++) {
      system("sleep 01");
      if (i == 0) {
        printf("\t*Para jogar novamente digita 1.\n");
      }
      if (i == 1) {
        printf("\t*Para voltar pro menu principal digita 2.\n");
      }
      if (i == 2) {
        printf("\t*Para voltar para menu inicial digita 3.\n");
      }
      if (i == 3) {
        printf(
            "\t*Para sair do jogo digita qualquer diferente de 1 ,2 e 3.\n\n");
      }
      if (i == 4) {
        printf("\t Digita aqui: ");
        scanf("%d", &jogar);
      }
      fflush(stdout);
    }
    if (jogar == 1) {
      system("clear");
      jogo(variacao, contagem + 1);
    }

    if (jogar == 2) {
      system("clear");
      menu_do_jogo();
    }
    if (jogar == 3) {
      system("clear");
      interface();
    } else {
      exit(0);
    }
  }
}

int main() {
  interface();
  return 0;
}
// FUNÇÃO JOGO AUTOMATICO
void jogo_automatico() {
  zerar();
  bombas();
  vizin_analisado();
  srand(time(NULL));
  int lin, colun, jogar;
  int control = 0;
  int caba = 0;
  int c, d;
  time_t begin = time(NULL);
  do {

    system("sleep 01");
    fflush(stdout);
    system("clear");
    campo();
    if (caba > 0) {
      printf("\n\tJogada feita: linha %d coluna %d.\n", lin, colun);
    }
    if (control == 0) {
      do {
        zerar();
        bombas();
        vizin_analisado();
        lin = rand() % 10;
        colun = rand() % 20;
      } while (Campo[lin][colun].minas == 1);
    }
    control = 1;
    do {
      for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 20; j++) {
          if (Campo[i][j].controle == 1 && Campo[i][j].vizinhos == 1) {
            c = i;
            d = j;
          }
        }
      }

      lin = rand() % 10;
      colun = rand() % 20;

    } while (coordenada_valida(lin, colun) == 0 ||
             Campo[lin][colun].minas == 1 || Campo[lin][colun].controle == 1);
    abrir(lin, colun);
    caba = 1;
  } while (ganhar() != 0 && Campo[lin][colun].minas == 0);

  if (Campo[lin][colun].minas == 1) {
    system("clear");
    campo();
    printa_texto("derrota.txt");
    printf("\n\tBomba encontrada!!!\n\tVocê perdeu!!!\n\n\n");
    time_t end = time(NULL);

    printf("\tO tempo gasto foi de %d segundos.\n", (end - begin));
    for (int i = 0; i < 5; i++) {
      system("sleep 01");
      if (i == 0) {
        printf("\t*Para jogar novamente digita 1.\n");
      }
      if (i == 1) {
        printf("\t*Para voltar pro menu inicial digita 2.\n");
      }
      if (i == 3) {
        printf("\t*Para sair do jogo digita qualquer diferente de 1 e 2.\n\n");
      }
      if (i == 4) {
        printf("\t Digita aqui: ");
        scanf("%d", &jogar);
      }
      fflush(stdout);
    }
    if (jogar == 1) {
      system("clear");
      menu_do_jogo();
    }
    if (jogar == 2) {
      system("clear");
      interface();
    } else {
      exit(0);
    }
  } else {
    system("clear");
    campo();
    printa_texto("vitoria.txt");
    printf("\n\tPARABÉNS!!!\n\tVocê ganhou!!!\n\n\n");
    time_t end = time(NULL);
    printf("\tO tempo gasto foi de %d segundos.\n", (end - begin));
    for (int i = 0; i < 5; i++) {
      system("sleep 01");
      if (i == 0) {
        printf("\t*Para jogar novamente digita 1.\n");
      }
      if (i == 1) {
        printf("\t*Para voltar pro menu principal digita 2.\n");
      }
      if (i == 2) {
        printf("\t*Para voltar para menu inicial digita 3.\n");
      }
      if (i == 3) {
        printf(
            "\t*Para sair do jogo digita qualquer diferente de 1 ,2 e 3.\n\n");
      }
      if (i == 4) {
        printf("\t Digita aqui: ");
        scanf("%d", &jogar);
      }
      fflush(stdout);
    }
    if (jogar == 1) {
      system("clear");
      jogo_automatico();
    }

    if (jogar == 2) {
      system("clear");
      menu_do_jogo();
    }
    if (jogar == 3) {
      system("clear");
      interface();
    } else {
      exit(0);
    }
  }
}
// FUNÇÃO AUXILIAR PARA A FUNÇÃO printa_texto FAZENDO USO DE MANIPULAÇÃO DE
// ARQUIVOS
void printa(FILE *file) {
  char *read_string;
  read_string = malloc(sizeof(char) * 128);
  while (fgets(read_string, sizeof(read_string), file) != NULL) {
    printf("%s", read_string);
  }
  free(read_string);
}

// Função de manipulação de arquivos para a impressão do .txt FAZENDO
// MANIPULAÇÃO DE ARQUIVOS
void printa_texto(char *filename) {
  FILE *file = NULL;
  file = fopen(filename, "r");
  printa(file);
  fclose(file);
}
