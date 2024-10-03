
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_PRODUTOS 10

typedef struct {
int codigo;
char nome[30];
float preco;
} Produto;

void carregarProdutos(Produto produtos[ ], int *total) {
FILE *file = fopen("produtos.txt", "r");
if (file == NULL) {
printf("Erro ao abrir o arquivo!\n");
return;
}

*total = 0;
while (fscanf(file, "%d %s %f", &produtos[*total].codigo, produtos[*total].nome, &produtos[*total].preco) != EOF) {
(*total)++;
}

fclose(file);
}

void listarProdutos(Produto produtos[], int total) {
printf("Lista de Produtos:\n");
for (int i = 0; i < total; i++) {
printf("Código: %d, Nome: %s, Preço: %.2f\n", produtos[i].codigo, produtos[i].nome, produtos[i].preco);
}
}

void adicionarProduto(Produto produtos[], int *total) {
if (*total >= MAX_PRODUTOS) {
printf("Limite de produtos atingido!\n");
return;
}

printf("Digite o código do produto: ");
scanf("%d", &produtos[*total].codigo);
printf("Digite o nome do produto: ");
scanf("%s", produtos[*total].nome);
printf("Digite o preço do produto: ");
scanf("%f", &produtos[*total].preco);

(*total)++;
}

void salvarProdutos(Produto produtos[], int total) {
FILE *file = fopen("produtos.txt", "w");
if (file == NULL) {
printf("Erro ao abrir o arquivo!\n");
return;
}

for (int i = 0; i < total; i++) {
fprintf(file, "%d %s %.2f\n", produtos[i].codigo, produtos[i].nome, produtos[i].preco);
}

fclose(file);
}

int main() {
Produto produtos[MAX_PRODUTOS];
int totalProdutos = 0;
int opcao;

carregarProdutos(produtos, &totalProdutos);

do {
printf("\nSistema de Mercado\n");
printf("1. Listar Produtos\n");
printf("2. Adicionar Produto\n");
printf("3. Sair\n");
printf("Escolha uma opção: ");
scanf("%d", &opcao);

switch (opcao) {
case 1:
listarProdutos(produtos, totalProdutos);
break;
case 2:
adicionarProduto(produtos, &totalProdutos);
salvarProdutos(produtos, totalProdutos);
break;
case 3:
printf("Saindo...\n");
break;
default:
printf("Opção inválida!\n");
}
} while (opcao != 3);

return 0;
}
