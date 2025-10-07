# Simulador-features
import json
import os

# Nome do arquivo para salvar os dados
ARQUIVO = "features.json"

# Função para carregar funcionalidades do arquivo
def carregar_features():
    if os.path.exists(ARQUIVO):
        with open(ARQUIVO, "r") as f:
            try:
                data = json.load(f)
                if isinstance(data, list):
                    return data
                else:
                    return []
            except json.JSONDecodeError:
                return []
    return []

# Função para salvar funcionalidades no arquivo
def salvar_features(features):
    with open(ARQUIVO, "w") as f:
        json.dump(features, f, indent=4)

# Lista de funcionalidades
features = carregar_features()

# Função para adicionar funcionalidade
def adicionar_features():
    nome = input("Nome da funcionalidade: ")
    valor = float(input("Valor para o usuário (1-10): "))
    esforco = float(input("Esforço necessário (1-10): "))
    pontuacao = valor / esforco
    feature = {"nome": nome, "valor": valor, "esforco": esforco, "pontuacao": pontuacao}
    features.append(feature)
    salvar_features(features)
    print(f"Funcionalidade '{nome}' adicionada com pontuação {pontuacao:.2f}\n")

# Função para listar funcionalidades por prioridade
def listar_features():
    if not features:
        print("Nenhuma funcionalidade cadastrada.\n")
        return
    features_ordenadas = sorted(features, key=lambda x: x["pontuacao"], reverse=True)
    print("=== Funcionalidades Prioritárias ===")
    for f in features_ordenadas:
        print(f"{f['nome']} - Valor: {f['valor']} | Esforço: {f['esforco']} | Pontuação: {f['pontuacao']:.2f}")
    print()
# Função para apagar funcionalidade
def apagar_features():
    if not features:
        print("Nenhuma funcionalidade cadastrada. \n")
        return
    print ("=== Funcionalidades Cadastradas ===")
    for i, f in enumerate (features, start=1):
       print (f"{i}. {f['nome']} - valor: {f['valor']} | Esforço: {f['esforco']}| Pontuação: {f['pontuacao']:.2f}")
    try:
        escolha = int (input("Digite o número da funcionalidade que deseja apagar: "))
        if 1 <= escolha <= len(features):
            removida= features.pop(escolha-1)
            salvar_features(features)
            print(f"Funcionalidade '{removida['nome']}' removida com sucesso! \n")
        else:
            print("Número inválido!\n")
    except ValueError:
        print("Entrada inválida! Digite apenas números. \n")


# Menu principal
def menu():
    while True:
        print("=== Simulador de Prioridade de Features ===")
        print("1. Adicionar funcionalidade")
        print("2. Listar funcionalidades")
        print("3. Apagar funcionalidade")
        print("4. Sair")
        escolha = input("Escolha uma opção: ")

        if escolha == "1":
            adicionar_features()
        elif escolha == "2":
            listar_features()
        elif escolha == "3":
            apagar_features()
        elif escolha == "4":
            print("Saindo...")
            break
        else:
            print("Opção inválida!\n")

# Executa o programa
if __name__ == "__main__":
    menu()
