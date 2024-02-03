pip install --user reportlab pandas

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
        y_position = height - 50  # Iniciar abaixo do topo da página

        # Para cada coluna, escrever a informação no PDF
        for col_name in data.columns:
            c.drawString(50, y_position, f"{col_name}: {row[col_name]}")
            y_position -= 20  # Mover para baixo para a próxima linha

            # Se chegarmos ao fundo da página, interrompemos para não sobrescrever a imagem
            if y_position < 100:
                break

        # Desenhar a imagem no final de cada página
        c.drawImage(image_path, width - 150, 50, width=100, preserveAspectRatio=True, anchor='se')

        # Finalizar a página atual e começar uma nova
        c.showPage()

    # Salvar o PDF
    c.save()

# Caminho para o arquivo CSV [Relative Path]
csv_file_path = 'abandonedCartsData/2024_02_02-cart-abandonment-recovery-export.csv'

# Caminho onde o PDF será salvo [Relative Path]
pdf_file_path = 'abandonedCartsPDF/output.pdf'

# Caminho para a imagem que será usada no PDF [Relative Path]
image_path = 'img/watermarkSignatureEAy.png'

# Criar o PDF
create_pdf(csv_file_path, pdf_file_path, image_path)

--------------------------------------------------------------------------------------------

