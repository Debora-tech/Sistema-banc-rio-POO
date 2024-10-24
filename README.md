class ContaBancaria:
    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.saldo = saldo

    def depositar(self, valor):
        if valor > 0:
            self.saldo += valor
            print(f"Depósito de R${valor:.2f} realizado com sucesso!")
        else:
            print("Valor inválido para depósito.")

    def sacar(self, valor):
        if 0 < valor <= self.saldo:
            self.saldo -= valor
            print(f"Saque de R${valor:.2f} realizado com sucesso!")
        else:
            print("Saldo insuficiente ou valor inválido para saque.")

    def verificar_saldo(self):
        print(f"Saldo atual de {self.titular}: R${self.saldo:.2f}")

    def transferir(self, valor, conta_destino):
        if 0 < valor <= self.saldo:
            self.saldo -= valor
            conta_destino.saldo += valor
            print(f"Transferência de R${valor:.2f} para {conta_destino.titular} realizada com sucesso!")
        else:
            print("Saldo insuficiente ou valor inválido para transferência.")


# Função para exibir o menu de opções
def exibir_menu():
    print("\n=== Sistema Bancário ===")
    print("1. Criar Conta")
    print("2. Depositar")
    print("3. Sacar")
    print("4. Verificar Saldo")
    print("5. Transferir")
    print("6. Exibir Todas as Contas")
    print("7. Sair")


# Função para buscar conta por nome do titular
def buscar_conta(contas, nome):
    return next((c for c in contas if c.titular == nome), None)


# Função principal
def main():
    contas = []

    while True:
        exibir_menu()
        opcao = input("Escolha uma opção: ")

        if opcao == '1':
            nome = input("Nome do titular: ")
            if buscar_conta(contas, nome):
                print(f"Já existe uma conta para o titular {nome}.")
            else:
                conta = ContaBancaria(nome)
                contas.append(conta)
                print(f"Conta criada com sucesso para {nome}!")

        elif opcao == '2':
            nome = input("Nome do titular: ")
            conta = buscar_conta(contas, nome)
            if conta:
                try:
                    valor = float(input("Valor a depositar: "))
                    conta.depositar(valor)
                except ValueError:
                    print("Por favor, insira um valor numérico.")
            else:
                print("Conta não encontrada.")

        elif opcao == '3':
            nome = input("Nome do titular: ")
            conta = buscar_conta(contas, nome)
            if conta:
                try:
                    valor = float(input("Valor a sacar: "))
                    conta.sacar(valor)
                except ValueError:
                    print("Por favor, insira um valor numérico.")
            else:
                print("Conta não encontrada.")

        elif opcao == '4':
            nome = input("Nome do titular: ")
            conta = buscar_conta(contas, nome)
            if conta:
                conta.verificar_saldo()
            else:
                print("Conta não encontrada.")

        elif opcao == '5':
            nome_origem = input("Nome do titular (Conta de origem): ")
            conta_origem = buscar_conta(contas, nome_origem)
            if conta_origem:
                nome_destino = input("Nome do titular (Conta de destino): ")
                conta_destino = buscar_conta(contas, nome_destino)
                if conta_destino:
                    try:
                        valor = float(input("Valor a transferir: "))
                        conta_origem.transferir(valor, conta_destino)
                    except ValueError:
                        print("Por favor, insira um valor numérico.")
                else:
                    print("Conta de destino não encontrada.")
            else:
                print("Conta de origem não encontrada.")

        elif opcao == '6':
            print("\n=== Lista de Contas ===")
            if contas:
                for conta in contas:
                    print(f"Titular: {conta.titular}, Saldo: R${conta.saldo:.2f}")
            else:
                print("Nenhuma conta cadastrada.")

        elif opcao == '7':
            print("Saindo do sistema bancário. Até logo!")
            break

        else:
            print("Opção inválida. Tente novamente.")


if __name__ == "__main__":
    main()
