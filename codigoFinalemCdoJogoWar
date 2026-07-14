#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

// Estrutura para representar um território
struct Territorio {
    char nome[30];
    char cor[10];
    int tropas;
};

// Estrutura para representar um jogador
struct Jogador {
    char cor[10];
    char *missao;  // Ponteiro para armazenar a missão dinamicamente
    int missaoCumprida;  // Flag para indicar se a missão foi cumprida
};

// Função para cadastrar os territórios
void cadastrarTerritorios(struct Territorio *mapa, int quantidade) {
    printf("\n=== CADASTRO DE TERRITORIOS ===\n");
    
    for (int i = 0; i < quantidade; i++) {
        printf("\nTerritorio %d:\n", i + 1);
        printf("Digite o nome do exercito: ");
        scanf("%s", (mapa + i)->nome);
        
        printf("Digite a cor do exercito: ");
        scanf("%s", (mapa + i)->cor);
        
        printf("Digite a quantidade de tropas: ");
        scanf("%d", &(mapa + i)->tropas);
    }
}

// Função para exibir todos os territórios
void exibirTerritorios(struct Territorio *mapa, int quantidade) {
    printf("\n=== TERRITORIOS ATUAIS ===\n");
    
    for (int i = 0; i < quantidade; i++) {
        printf("\nTerritorio %d:\n", i + 1);
        printf("  Nome: %s\n", (mapa + i)->nome);
        printf("  Cor: %s\n", (mapa + i)->cor);
        printf("  Tropas: %d\n", (mapa + i)->tropas);
    }
}

// Função que implementa o ataque entre territórios
void atacar(struct Territorio *atacante, struct Territorio *defensor) {
    int dadoAtacante = (rand() % 6) + 1;
    int dadoDefensor = (rand() % 6) + 1;
    
    printf("\n=== RESULTADO DO ATAQUE ===\n");
    printf("Dado do atacante: %d\n", dadoAtacante);
    printf("Dado do defensor: %d\n", dadoDefensor);
    
    if (dadoAtacante > dadoDefensor) {
        printf("O ATACANTE VENCEU!\n");
        printf("Territorio %s agora pertence a %s\n", defensor->nome, atacante->cor);
        
        int tropasTransferidas = atacante->tropas / 2;
        atacante->tropas -= tropasTransferidas;
        
        strcpy(defensor->cor, atacante->cor);
        defensor->tropas = tropasTransferidas;
        
        printf("Tropas transferidas: %d\n", tropasTransferidas);
    } else {
        printf("O DEFENSOR VENCEU!\n");
        printf("O atacante perdeu 1 tropa\n");
        
        if (atacante->tropas > 0) {
            atacante->tropas--;
        }
    }
}

// Função para validar se o ataque é válido
int validarAtaque(struct Territorio *atacante, struct Territorio *defensor) {
    if (strcmp(atacante->cor, defensor->cor) == 0) {
        printf("ERRO: Nao pode atacar um territorio da mesma cor!\n");
        return 0;
    }
    return 1;
}

// Função para atribuir uma missão aleatória ao jogador
// destino: ponteiro para onde a missão será copiada (passagem por referência)
// missoes: vetor de strings com as missões disponíveis
// totalMissoes: quantidade total de missões no vetor
void atribuirMissao(char *destino, char *missoes[], int totalMissoes) {
    int indiceSorteado = rand() % totalMissoes;
    strcpy(destino, missoes[indiceSorteado]);
}

// Função para exibir a missão do jogador (passagem por valor)
void exibirMissao(char missao[]) {
    printf("Sua missao secreta: %s\n", missao);
}

// Função para verificar se um jogador cumpriu sua missão
// Retorna 1 se cumpriu, 0 caso contrário
int verificarMissao(char *missao, struct Territorio *mapa, int tamanho, char *corJogador) {
    int territoriosConquistados = 0;
    int territoriosTotal = 0;
    
    // Conta quantos territórios o jogador possui
    for (int i = 0; i < tamanho; i++) {
        if (strcmp((mapa + i)->cor, corJogador) == 0) {
            territoriosConquistados++;
        }
        territoriosTotal++;
    }
    
    // Verifica diferentes tipos de missão
    // Missão 1: Conquistar todos os territórios
    if (strstr(missao, "todos os territorios") != NULL) {
        return territoriosConquistados == territoriosTotal;
    }
    
    // Missão 2: Conquistar pelo menos 3 territórios
    if (strstr(missao, "3 territorios") != NULL || 
        strstr(missao, "tres territorios") != NULL) {
        return territoriosConquistados >= 3;
    }
    
    // Missão 3: Conquistar pelo menos 2 territórios
    if (strstr(missao, "2 territorios") != NULL || 
        strstr(missao, "dois territorios") != NULL) {
        return territoriosConquistados >= 2;
    }
    
    // Missão 4: Eliminar todos os territórios de uma cor específica
    if (strstr(missao, "Eliminar") != NULL) {
        // Extrai a cor a ser eliminada
        char corAlvo[10];
        // Procura pela palavra "cor" na missão e extrai a cor
        char *ptr = strstr(missao, "cor ");
        if (ptr != NULL) {
            sscanf(ptr, "cor %s", corAlvo);
            // Remove pontuação se houver
            for (int i = 0; corAlvo[i]; i++) {
                if (corAlvo[i] == '.' || corAlvo[i] == ',') {
                    corAlvo[i] = '\0';
                    break;
                }
            }
            
            // Verifica se ainda existe algum território com essa cor
            int territoriosDaCor = 0;
            for (int i = 0; i < tamanho; i++) {
                if (strcmp((mapa + i)->cor, corAlvo) == 0) {
                    territoriosDaCor++;
                }
            }
            return territoriosDaCor == 0;
        }
    }
    
    // Missão 5: Conquistar territórios seguidos (verificação simplificada)
    if (strstr(missao, "seguidos") != NULL) {
        int maxSequencia = 0;
        int sequenciaAtual = 0;
        
        for (int i = 0; i < tamanho; i++) {
            if (strcmp((mapa + i)->cor, corJogador) == 0) {
                sequenciaAtual++;
                if (sequenciaAtual > maxSequencia) {
                    maxSequencia = sequenciaAtual;
                }
            } else {
                sequenciaAtual = 0;
            }
        }
        
        // Verifica se conquistou 3 territórios seguidos
        return maxSequencia >= 3;
    }
    
    // Se não reconhecer a missão, retorna 0
    return 0;
}

// Função para exibir o status das missões de todos os jogadores
void exibirStatusMissoes(struct Jogador *jogadores, int numJogadores) {
    printf("\n=== STATUS DAS MISSOES ===\n");
    for (int i = 0; i < numJogadores; i++) {
        printf("Jogador %s: ", jogadores[i].cor);
        if (jogadores[i].missaoCumprida) {
            printf("MISSAO CUMPRIDA! 🏆\n");
        } else {
            printf("Missao em andamento\n");
        }
    }
}

// Função para liberar toda a memória alocada
void liberarMemoria(struct Territorio *mapa, struct Jogador *jogadores, int numJogadores) {
    // Libera a missão de cada jogador
    for (int i = 0; i < numJogadores; i++) {
        free(jogadores[i].missao);
    }
    
    // Libera o vetor de jogadores
    free(jogadores);
    
    // Libera o mapa de territórios
    free(mapa);
    
    printf("\nMemoria liberada com sucesso!\n");
}

int main() {
    int quantidadeTerritorios;
    int numJogadores;
    
    // Inicializa o gerador de números aleatórios
    srand(time(NULL));
    
    printf("=== JOGO WAR - NIVEL MESTRE ===\n");
    
    // Define o número de territórios
    printf("Digite a quantidade total de territorios: ");
    scanf("%d", &quantidadeTerritorios);
    
    // Define o número de jogadores
    printf("Digite o numero de jogadores: ");
    scanf("%d", &numJogadores);
    
    // Alocação dinâmica do mapa de territórios
    struct Territorio *mapa = (struct Territorio *)calloc(quantidadeTerritorios, 
                                                           sizeof(struct Territorio));
    if (mapa == NULL) {
        printf("ERRO: Falha na alocação de memória para territórios!\n");
        return 1;
    }
    
    // Alocação dinâmica do vetor de jogadores
    struct Jogador *jogadores = (struct Jogador *)calloc(numJogadores, 
                                                          sizeof(struct Jogador));
    if (jogadores == NULL) {
        printf("ERRO: Falha na alocação de memória para jogadores!\n");
        free(mapa);
        return 1;
    }
    
    // Cadastra os territórios
    cadastrarTerritorios(mapa, quantidadeTerritorios);
    
    // Define as cores dos jogadores baseado nos territórios cadastrados
    printf("\n=== DEFININDO JOGADORES ===\n");
    for (int i = 0; i < numJogadores && i < quantidadeTerritorios; i++) {
        strcpy(jogadores[i].cor, (mapa + i)->cor);
        printf("Jogador %d definido com a cor: %s\n", i + 1, jogadores[i].cor);
    }
    
    // Vetor de missões disponíveis
    char *missoes[] = {
        "Conquistar todos os territorios",
        "Conquistar pelo menos 3 territorios",
        "Conquistar pelo menos 2 territorios",
        "Conquistar 3 territorios seguidos",
        "Eliminar todos os territorios de cor vermelha"
    };
    int totalMissoes = 5;
    
    // Atribui missões aleatórias para cada jogador
    printf("\n=== ATRIBUINDO MISSOES ===\n");
    for (int i = 0; i < numJogadores; i++) {
        // Aloca memória para a missão do jogador
        jogadores[i].missao = (char *)malloc(100 * sizeof(char));
        if (jogadores[i].missao == NULL) {
            printf("ERRO: Falha na alocação de memória para missão!\n");
            liberarMemoria(mapa, jogadores, i);  // Libera o que já foi alocado
            return 1;
        }
        
        // Atribui a missão (passagem por referência)
        atribuirMissao(jogadores[i].missao, missoes, totalMissoes);
        jogadores[i].missaoCumprida = 0;
        
        // Exibe a missão para o jogador (passagem por valor)
        printf("\nJogador %s:\n", jogadores[i].cor);
        exibirMissao(jogadores[i].missao);
        printf("------------------------\n");
    }
    
    // Exibe os territórios cadastrados
    exibirTerritorios(mapa, quantidadeTerritorios);
    
    // Menu principal do jogo
    int opcao;
    int jogoAtivo = 1;
    
    do {
        printf("\n=== MENU DE BATALHA ===\n");
        printf("1 - Realizar ataque\n");
        printf("2 - Exibir territorios\n");
        printf("3 - Verificar missoes\n");
        printf("0 - Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);
        
        switch (opcao) {
            case 1: {
                if (!jogoAtivo) {
                    printf("O jogo ja terminou! Reinicie para jogar novamente.\n");
                    break;
                }
                
                int atacanteIdx, defensorIdx;
                
                printf("\nEscolha o territorio ATACANTE (0 a %d): ", quantidadeTerritorios - 1);
                scanf("%d", &atacanteIdx);
                
                printf("Escolha o territorio DEFENSOR (0 a %d): ", quantidadeTerritorios - 1);
                scanf("%d", &defensorIdx);
                
                // Validações
                if (atacanteIdx < 0 || atacanteIdx >= quantidadeTerritorios || 
                    defensorIdx < 0 || defensorIdx >= quantidadeTerritorios) {
                    printf("ERRO: Indice de territorio invalido!\n");
                    break;
                }
                
                if (atacanteIdx == defensorIdx) {
                    printf("ERRO: Nao pode atacar o mesmo territorio!\n");
                    break;
                }
                
                if (!validarAtaque(mapa + atacanteIdx, mapa + defensorIdx)) {
                    break;
                }
                
                if ((mapa + atacanteIdx)->tropas <= 0) {
                    printf("ERRO: Territorio atacante sem tropas!\n");
                    break;
                }
                
                // Realiza o ataque
                atacar(mapa + atacanteIdx, mapa + defensorIdx);
                
                // Verifica missões após cada ataque
                printf("\n=== VERIFICANDO MISSOES ===\n");
                int alguemVenceu = 0;
                
                for (int i = 0; i < numJogadores; i++) {
                    if (!jogadores[i].missaoCumprida) {
                        if (verificarMissao(jogadores[i].missao, mapa, 
                                           quantidadeTerritorios, jogadores[i].cor)) {
                            jogadores[i].missaoCumprida = 1;
                            printf("🏆 JOGADOR %s CUMPRIU SUA MISSAO! 🏆\n", jogadores[i].cor);
                            alguemVenceu = 1;
                        }
                    }
                }
                
                if (alguemVenceu) {
                    printf("\n=== FIM DE JOGO! ===\n");
                    exibirStatusMissoes(jogadores, numJogadores);
                    jogoAtivo = 0;
                }
                
                exibirTerritorios(mapa, quantidadeTerritorios);
                break;
            }
            
            case 2:
                exibirTerritorios(mapa, quantidadeTerritorios);
                break;
            
            case 3:
                exibirStatusMissoes(jogadores, numJogadores);
                break;
                
            case 0:
                printf("Saindo do jogo...\n");
                break;
                
            default:
                printf("Opcao invalida!\n");
        }
        
    } while (opcao != 0);
    
    // Libera toda a memória alocada
    liberarMemoria(mapa, jogadores, numJogadores);
    
    return 0;
}

