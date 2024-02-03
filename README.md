<!-- pip install --user reportlab pandas

--------------------------------------------------------------------------------------------

import pandas as pd
from reportlab.lib.pagesizes import letter
from reportlab.pdfgen import canvas
from reportlab.lib import colors

def create_pdf(csv_file_path, pdf_file_path, image_path):
    # Carregar os dados do arquivo CSV
    data = pd.read_csv(csv_file_path)

    # Iniciar o objeto canvas para o PDF
    c = canvas.Canvas(pdf_file_path, pagesize=letter)
    width, height = letter  # Tamanho da página

    for index, row in data.iterrows():
        # Título no início da página
        c.setFont("Helvetica-Bold", 18)  # Fonte maior para o tópico
        title = "Sales Recovery - eCommerce Elite Aço"
        title_width = c.stringWidth(title, "Helvetica-Bold", 16)
        c.drawCentredString(width / 2, height - 40, title)  # Centralizado no topo
        y_position = height - 40 - 5 * 12  # Duas linhas de espaço após o tópico

        c.setFont("Helvetica", 10)  # Resetando a fonte para o texto normal

        # Para cada coluna, escrever a informação no PDF
        for col_name in data.columns:
            c.drawString(50, y_position, f"{col_name}: {row[col_name]}")
            y_position -= 20  # Mover para baixo para a próxima linha

            # Se chegarmos próximo ao fundo da página, paramos para deixar espaço para o texto adicional
            if y_position < 250:
                break

        # Linha de separação
        c.line(50, y_position - 10, width - 50, y_position - 10)

        # Textos adicionais com formatação
        y_position -= 50  # Espaço antes do texto FOLLOW UP
        c.setFont("Helvetica-Bold", 14)
        c.drawString(50, y_position, "FOLLOW UP")
        c.setFont("Helvetica", 10)  # Resetando a fonte para o texto normal
        follow_up_texts = [
            "- CONTATO ADICIONADO?    [   ] SIM  [   ] NÃO",
            "- CONTATO FEITO?    [   ] SIM  [   ] NÃO",
            "- QUANDO?    ____ / ____ / _______",
            "- CLIENTE RESPONDEU?    [   ] SIM  [   ] NÃO",
            "- CONSEGUIMOS RECUPERAR A VENDA?    [   ] SIM  [   ] NÃO",
            "- SE SIM, EM QUAL PEDIDO?   ____________."
        ]

        # Desenhando cada item do FOLLOW UP
        for text in follow_up_texts:
            y_position -= 40  # Espaço maior para pular duas linhas
            c.drawString(50, y_position, text)

        # Após o último item, adicionar um espaço extra
        y_position -= 20  # Adiciona espaço extra após o último item de FOLLOW UP

        # Desenhar a imagem no final de cada página
        c.drawImage(image_path, width - 150, 50, width=100, preserveAspectRatio=True, anchor='se')

        # Finalizar a página atual e começar uma nova
        c.showPage()

    # Salvar o PDF
    c.save()

# Caminho para o arquivo CSV [Relative Path]
csv_file_path = 'abandonedCartsData/2024_02_02-cart-abandonment-recovery-export.csv'

# Caminho onde o PDF será salvo [Relative Path]
pdf_file_path = 'abandonedCartsPDF/output7.pdf'

# Caminho para a imagem que será usada no PDF [Relative Path]
image_path = 'img/watermarkSignatureEAy.png'

# Criar o PDF
create_pdf(csv_file_path, pdf_file_path, image_path)


--------------------------------------------------------------------------------------------


 -->
