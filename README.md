EXPOTECH

import mysql.connector
from datetime import datetime

conexao = mysql.connector.connect(host='localhost', user='root', password='root', database='gest_de_estoque')
cursor = conexao.cursor()

def cadastrar_produto():
    nome_produt = input("Digite o nome do produto: ").strip()
    preco_produt = float(input("Digite o preço do produto: "))
    print("\nCategorias disponíveis:")
    cursor.execute("SELECT id_estoque, nome_estoque FROM estoque")
    resultado = cursor.fetchall()
    
    for linha in resultado:
        print(f"ID: {linha[0]} - Tipo: {linha[1]}")
    posicao_produt = (input("Digite o ID do estoque: "))
    print("\nCategorias disponíveis:")
    cursor.execute("SELECT id_categoria, tipo_categoria FROM categoria")
    resultado = cursor.fetchall()
    
    for linha in resultado:
        print(f"ID: {linha[0]} - Tipo: {linha[1]}")

    cat_produt = input("Digite o ID da categoria do produto: ").strip()
    sql = 'INSERT INTO produtos (nome_produt, preco_produt,fk_estoque_id_estoque,fk_categoria_id_categoria) VALUES (%s,%s,%s,%s)'
    valores = (nome_produt,preco_produt,posicao_produt,cat_produt)
    cursor.execute(sql, valores)  
    conexao.commit()
    print(f"Produto '{nome_produt}' cadastrado com sucesso!")
    



def remover_produto():
    ex = int(input("Digite o ID do produto a ser excluído: "))
    sql = "DELETE FROM produto WHERE id_prod = %s"
    valores = (ex,)
    cursor.execute(sql,valores)
    conexao.commit()
    print(cursor.rowcount, "registro(s) excluído(s).")




def atualizar_preco():
    nome_produto = input("Digite o nome: ")
    id_produto = int(input("Digite o código do usuário a ser alterado: "))
    sql = "UPDATE produto SET nome_produto = %s WHERE id_prod = %s"
    valores = (nome_produto, id_produto)
    cursor.execute(sql,valores)
    conexao.commit()



def listar_produtos():
    cursor.execute("SELECT * FROM produtos")
    resultado = cursor.fetchall()
    for linha in resultado:
        nome_produto = linha[1]
        valor_produto = linha[2]
       
        print(f"\n-------------------------\nNome do produto: {nome_produto}\nValor: {valor_produto}\n-------------------------\n")



def menu_prod():
    while True:
        print("\n--- MENU DE PRODUTOS ---")
        print("1. Cadastrar produto")
        print("2. Remover produto")
        print("3. Atualizar preço")
        print("4. Listar produtos")
        print("5. Sair")




        opcao = input("Escolha uma opção: ")




        if opcao == "1":
            cadastrar_produto()
        elif opcao == "2":
            remover_produto()
        elif opcao == "3":
            atualizar_preco()
        elif opcao == "4":
            listar_produtos()
        elif opcao == "5":
            print("Saindo do sistema. Até logo!")
            break
        else:
            print("Opção inválida. Tente novamente.")





def cadastrar_estoque():
    nome_estoque = input("Digite o item deste estoque: ").strip()
    
    try:
        quant_Max = float(input("Digite a quantidade máxima usada: "))
        quant_Min = float(input("Digite a quantidade mínima usada: "))
    except ValueError:
        print("Por favor, digite um número válido para as quantidades.")
        return

    data_entrada = input("Digite a data atual (dd/mm/aaaa): ")
    
    try:
        data = datetime.strptime(data_entrada, "%d/%m/%Y")
    except ValueError:
        print("Data inválida. Use o formato dd/mm/aaaa.")
        return


    data_entrada_conv = data.strftime("%Y-%m-%d")
    cursor = conexao.cursor()


    cursor.execute("SELECT * FROM funcionario")
    resultado = cursor.fetchall()
    for linha in resultado:
        id_funcionario = linha[0]
        nome_funcionario = linha[1]
        rg_funcionario = linha[2]
       
        print(f"\nID funcionario:{id_funcionario}\nNome do funcionario:{nome_funcionario}\nRG: {rg_funcionario}\n")


    fk_funcionario_id_funcionario = input("Quem e o responsavel por este estoque?\n")
    
    sql = 'INSERT INTO estoque (nome_estoque, quant_Max, quant_Min, data_entrada,fk_funcionario_id_funcionario) VALUES (%s, %s, %s, %s,%s)'
    valores = (nome_estoque, quant_Max, quant_Min, data_entrada_conv,fk_funcionario_id_funcionario)
    cursor.execute(sql, valores)  
    conexao.commit()
    print(f"Seu estoque '{nome_estoque}' foi criado com sucesso!")






def remover_estoque():
    cursor.execute("SELECT * FROM estoque")
    resultado = cursor.fetchall()
    for linha in resultado:
        id_estoque = linha[0]
        nome_estoque = linha[1]
        print(f"\nID estoque:{id_estoque}\nNome do estoque:{nome_estoque}\n")

    ex = int(input("Digite o ID do estoque a ser excluído: "))
    sql = "DELETE FROM estoque WHERE id_estoque = %s"
    valores = (ex,)
    cursor.execute(sql,valores)
    conexao.commit()
    print(cursor.rowcount, "registro(s) excluído(s).")




def atualizar_estoque():
    nome_novo_estoque = input("Digite o novo nome para o estoque: ")


    cursor.execute("SELECT * FROM estoque")
    estoques = cursor.fetchall()
    for linha in estoques:
        print(f"\nID estoque: {linha[0]}\nNome do estoque: {linha[1]}\n")

    id_estoque = int(input("Digite o ID do estoque a ser alterado: "))
    quant_Max = float(input("Digite a quantidade máxima usada: "))
    quant_Min = float(input("Digite a quantidade mínima usada: "))


    cursor.execute("SELECT * FROM funcionario")
    funcionarios = cursor.fetchall()
    for linha in funcionarios:
        print(f"\nID funcionário: {linha[0]}\nNome: {linha[1]}\nRG: {linha[2]}\n")

    fk_funcionario_id_funcionario = int(input("Digite o ID do funcionário responsável por este estoque: "))

  
    sql = """
        UPDATE estoque
        SET nome_estoque = %s, quant_Max = %s, quant_Min = %s, fk_funcionario_id_funcionario = %s
        WHERE id_estoque = %s
    """
    valores = (nome_novo_estoque, quant_Max, quant_Min, fk_funcionario_id_funcionario, id_estoque)
    cursor.execute(sql, valores)
    conexao.commit()

    print(f"Estoque '{nome_novo_estoque}' atualizado com sucesso!")


def listar_estoque():
    cursor.execute("SELECT * FROM estoque")
    resultado = cursor.fetchall()
    for linha in resultado:
        id_estoque = linha[0]
        nome_estoque = linha[1]
       
        print(f"\n-------------------------\nNome do produto: {id_estoque}\nValor: {nome_estoque}\n-------------------------\n")

        escolha=int(input("Gostaria de pesquisar um estoque?\n1 - Sim\n2 - Não"))
        if escolha == 1:
      
            id_busca = input("Digite o ID do estoque: ")

            sql = "SELECT * FROM estoque WHERE id_estoque = %s"
            cursor.execute(sql, (id_busca,))

            resultado = cursor.fetchone()

            if resultado:
                print("Estoque encontrado:")
                print(resultado)
            else:
                print("Nenhum estoque encontrado com esse ID.")

            cursor.close()
            conexao.close()
        elif escolha == 2:
            break
        else:
            print("Opção invalida!")
            






def menu_estoq():
    while True:
        print("\n--- MENU DE ESTOQUE ---")
        print("1. Criar estoque")
        print("2. Remover estoque")
        print("3. Atualizar estoque")
        print("4. Listar estoque")
        print("5. Sair")




        opcao = input("Escolha uma opção: ")




        if opcao == "1":
            cadastrar_estoque()
        elif opcao == "2":
            remover_estoque()
        elif opcao == "3":
            atualizar_estoque()
        elif opcao == "4":
            listar_estoque()
        elif opcao == "5":
            break
        else:
            print("Opção inválida. Tente novamente.")






def cadastrar_funcionario():
    nome_funcionario = (input("Digite o nome do produto: ")).strip()
    rg_funcionario = (input("Digite o preço do produto: ")).strip()
    cracha = (input("Digite o ID do estoque: ")).strip()

    sql = 'INSERT INTO funcionario (nome_funcionario, rg_funcionario, cracha) VALUES (%s,%s,%s)'
    valores = (nome_funcionario,rg_funcionario,cracha)
    cursor.execute(sql, valores)  
    conexao.commit()
    print(f"Funcionario '{nome_funcionario}' cadastrado com sucesso!")




def remover_funcionario():
    ex = int(input("Digite o ID do funcionario a ser excluído: "))
    sql = "DELETE FROM funcionario WHERE id_funcionario = %s"
    valores = (ex,)
    cursor.execute(sql,valores)
    conexao.commit()
    print(cursor.rowcount, "registro(s) excluído(s).")




def atualizar_funcionario():
    nome_funcionario = input("Digite o nome: ")
    id_funcionario = int(input("Digite o código do usuário a ser alterado: "))
    sql = "UPDATE funcionario SET nome_funcionario = %s WHERE id_funcionario = %s"
    valores = (nome_funcionario, id_funcionario)
    cursor.execute(sql,valores)
    conexao.commit()




def menu_func():
    while True:
        print("\n--- MENU DE FUNCIONARIOS ---")
        print("1. Cadastrar funcionario")
        print("2. Remover funcionario")
        print("3. Atualizar dados do funcionario")
        print("4. Sair")




        opcao = input("Escolha uma opção: ")




        if opcao == "1":
            cadastrar_funcionario()
        elif opcao == "2":
            remover_funcionario()
        elif opcao == "3":
            atualizar_funcionario()
        elif opcao == "4":
            break
        else:
            print("Opção inválida. Tente novamente.")



def criar_categoria():
    tipo_categoria = (input("Digite a nova categoria:\n"))
    sql = 'INSERT INTO categoria (tipo_categoria) VALUES (%s)'
    valores = (tipo_categoria,)
    cursor.execute(sql, valores)  
    conexao.commit()
    print(f"Categoria '{tipo_categoria}' criada com sucesso!")





def menu():
    while True:
        print("\nBem vindo ao StockSys!\n(Sistema de Gerenciamento de Estoque Gerais)")
        print("O que gostaria de fazer hoje?\n")
        print("1. Menu de Produtos")
        print("2. Menu de Estoque")
        print("3. Menu de Funcionarios")
        print("4. Cria um categoria de produto")
        print("5. Sair")

        opcao = input("Escolha uma opção: ")




        if opcao == "1":
                menu_prod()
        elif opcao == "2":
                menu_estoq()
        elif opcao == "3":
                menu_func()
        elif opcao == "4":
                criar_categoria()
        elif opcao == "5":
            print("Saindo do sistema. Até logo!")
            break
        else:
            print("Opção inválida. Tente novamente.")

menu()
