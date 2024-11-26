class Cliente:
    def __init__(self, id, nome, cpf, idade) -> None:
        self.id = id
        self.nome = nome
        self.cpf = cpf
        self.idade = idade


class Quarto:
    def __init__(self, id, nome, valor, disponibilidade) -> None:
        self.id = id
        self.nome = nome
        self.valor = valor
        self.disponibilidade = disponibilidade


class Reserva:
    def __init__(self, id, data_entrada, quarto, cliente, data_saida) -> None:
        self.id = id
        self.data_entrada = data_entrada
        self.data_saida = data_saida
        self.quarto = quarto
        self.cliente = cliente


class Hotel:
    def __init__(self) -> None:
        self.idclientes = 1
        self.idreservas = 1
        self.idquartos = 1
        self.lista_de_quartos = []
        self.lista_de_clientes = []
        self.lista_de_reservas = []

    def adicionar_cliente(self):
        nome = input("Digite o nome do cliente: ")
        cpf = input("Digite o CPF do cliente: ")
        idade = int(input("Digite a idade do cliente: "))
        
        cliente = Cliente(self.idclientes, nome, cpf, idade)
        self.idclientes += 1
        self.lista_de_clientes.append(cliente)
        print("Cliente adicionado com sucesso!")

    def adicionar_quarto(self):
        nome = input("Digite o nome do quarto: ")
        valor = float(input("Digite o valor do quarto: "))
        disponibilidade = input("O quarto está disponível? (S/N): ").strip().lower() == 's'
        
        quarto = Quarto(self.idquartos, nome, valor, disponibilidade)
        self.idquartos += 1
        self.lista_de_quartos.append(quarto)
        print("Quarto adicionado com sucesso!")

    def ver_quartos_disponiveis(self):
        print("Quartos disponíveis:")
        for quarto in self.lista_de_quartos:
            if quarto.disponibilidade:
                print(f"ID: {quarto.id}, Nome: {quarto.nome}, Valor: {quarto.valor}")
        print("")

    def ver_quartos_reservados(self):
        print("Quartos reservados:")
        for reserva in self.lista_de_reservas:
            print(f"ID: {reserva.quarto.id}, Nome: {reserva.quarto.nome}, Valor: {reserva.quarto.valor}")
        print("")

    def fazer_reserva(self):
        cliente_id = int(input("Digite o ID do cliente que fará a reserva: "))
        quarto_id = int(input("Digite o ID do quarto a ser reservado: "))
        data_entrada = input("Digite a data de entrada (dd/mm/aaaa): ")
        data_saida = input("Digite a data de saída (dd/mm/aaaa): ")
        
        cliente = next((c for c in self.lista_de_clientes if c.id == cliente_id), None)
        quarto = next((q for q in self.lista_de_quartos if q.id == quarto_id), None)

        if cliente and quarto and quarto.disponibilidade:
            reserva = Reserva(self.idreservas, data_entrada, quarto, cliente, data_saida)
            self.lista_de_reservas.append(reserva)
            quarto.disponibilidade = False
            self.idreservas += 1
            print("Reserva realizada com sucesso!")
        else:
            print("Erro: Cliente ou quarto inválido, ou quarto já reservado.")

    def encerrar_reserva(self):
        reserva_id = int(input("Digite o ID da reserva a ser encerrada: "))
        reserva = next((r for r in self.lista_de_reservas if r.id == reserva_id), None)
        
        if reserva:
            reserva.quarto.disponibilidade = True
            self.lista_de_reservas.remove(reserva)
            print("Reserva encerrada com sucesso!")
        else:
            print("Erro: Reserva não encontrada.")


hotel = Hotel()

while True:
    menu = input("""
MENU DO HOTEL:
1 - Ver quartos disponíveis
2 - Ver quartos reservados
3 - Fazer uma reserva
4 - Encerrar uma reserva
5 - Gerenciar Clientes
6 - Gerenciar Quartos
0 - Sair
Escolha uma opção: """)
    
    match menu:
        case "1":
            hotel.ver_quartos_disponiveis()
        case "2":
            hotel.ver_quartos_reservados()
        case "3":
            hotel.fazer_reserva()
        case "4":
            hotel.encerrar_reserva()
        case "5":
            hotel.adicionar_cliente()
        case "6":
            hotel.adicionar_quarto()
        case "0":
            print("Programa encerrado!")
            break
        case _:
            print("Opção inválida! Por favor, selecione uma opção válida.")
