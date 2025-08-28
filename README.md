# banktech
import textwrap

def menu():
    menu_text = """
    Bem vindo ao BankTech.
    [1] Depositar
    [2] Sacar
    [3] Extrato
    [4] Sair
    ---->  """
    return input(textwrap.dedent(menu_text))

def depositar(saldo, valor, extrato, /):
    if valor > 0:
        saldo += valor
        extrato += f"Depósito: R$ {valor:.2f}\n"
        print("\nDepósito realizado com sucesso!")
    else:
        print("\nOperação falhou! O valor informado é inválido.")
    return saldo, extrato

def sacar(*, saldo, valor, extrato, limite, numero_saques, limite_saques):
    exc_saldo = valor > saldo
    exc_limite = valor > limite
    exc_saques = numero_saques >= limite_saques

    if exc_saldo:
        print("\nOperação falhou! Você não tem saldo suficiente.")
    elif exc_limite:
        print("\nOperação falhou! O valor do saque excede o limite.")
    elif exc_saques:
        print("\nOperação falhou! Número máximo de saques diários atingido.")
    elif valor > 0:
        saldo -= valor
        extrato += f"Saque: R$ {valor:.2f}\n"
        numero_saques += 1
        print("\nSaque realizado com sucesso!")
    else:
        print("\nOperação falhou! O valor informado é inválido.")
    
    return saldo, extrato, numero_saques

def exibir_extrato(saldo, /, *, extrato):
    print("\n================ EXTRATO ================")
    print("Não foram realizadas movimentações." if not extrato else extrato)
    print(f"\nSaldo: R$ {saldo:.2f}")
    print("==========================================")

def main():

    LIMITE_SAQUES = 3
    AGENCIA = "0001"

    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0

    while True:
        opcao = menu()

        if opcao == "d":
            valor = float(input("Informe o valor do depósito: "))
            saldo, extrato = depositar(saldo, valor, extrato)

        elif opcao == "s":
            valor = float(input("Informe o valor do saque: "))
            saldo, extrato, numero_saques = sacar(
                saldo=saldo,
                valor=valor,
                extrato=extrato,
                limite=limite,
                numero_saques=numero_saques,
                limite_saques=LIMITE_SAQUES,
            )

        elif opcao == "e":
            exibir_extrato(saldo, extrato=extrato)

        elif opcao == "q":
            print("Saindo do sistema. Obrigado por usar nosso banco!")
            break

        else:
            print("Operação inválida, por favor selecione novamente a operação desejada.")

if __name__ == "__main__":
    main()
