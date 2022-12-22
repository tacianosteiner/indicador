# indicador ##volume/divergencia/convergencia


#importação das bibliotecas necessárias
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd

#função para calcular oscilador estocástico
def oscilador_estocastico(preco_fechamento, preco_maximo, preco_minimo, periodo):
#cálculo da média móvel
media_movel_k = 100*(preco_fechamento - preco_minimo)/(preco_maximo - preco_minimo)
#cálculo das 3 médias móveis exponenciais
media_movel_k_3 = media_movel_k.ewm(com=3).mean()
media_movel_k_7 = media_movel_k.ewm(com=7).mean()
media_movel_k_14 = media_movel_k.ewm(com=14).mean()

#criação do gráfico
plt.plot(media_movel_k, label="Oscilador Estocástico")
plt.plot(media_movel_k_3, label="Média Móvel Exponencial (com=3)")
plt.plot(media_movel_k_7, label="Média Móvel Exponencial (com=7)")
plt.plot(media_movel_k_14, label="Média Móvel Exponencial (com=14)")
plt.legend(loc="upper left")
plt.show()

#cálculo do histograma de volume
volume = pd.DataFrame(preco_fechamento).diff()
volume.fillna(0, inplace=True)
volume_positivo = volume[volume > 0].sum()
volume_negativo = volume[volume < 0].sum()

#plotagem do histograma
plt.bar(["Volume Positivo", "Volume Negativo"], [volume_positivo, volume_negativo], color=["green", "red"])
plt.show()

#detecção de divergências e convergências
if media_movel_k > media_movel_k_3 and media_movel_k_3 > media_movel_k_7 and media_movel_k_7 > media_movel_k_14:
    plt.plot([preco_fechamento.index[0], preco_fechamento.index[-1]], [media_movel_k[0], media_movel_k[-1]], color="green")
    plt.show()
    print("Convergência detectada")
elif media_move3_k < media_movel_k_3 and media_movel_k_7 < media_movel_k_7 and media_movel_k_14 < media_movel_k
