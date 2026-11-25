# Rastreamento e contagem de veículos com YOLO

Projeto do Grupo 7 para o Desafio da Semana 4 da unidade de Visão Computacional da Residência em Inteligência Artificial.

O sistema detecta e rastreia veículos em vídeos de trânsito. A contagem aumenta quando o centro de um veículo rastreado cruza uma linha virtual definida para cada vídeo.

## Como funciona

1. O notebook usa quatro vídeos de trânsito baixados pelo KaggleHub. [Road Traffic Video Monitoring](https://www.kaggle.com/datasets/shawon10/road-traffic-video-monitoring) · [Vehicle Detection Sample and Output Videos](https://www.kaggle.com/datasets/vivek603/vehicle-detection-sample-and-output-videos)
2. O YOLO11m pré-treinado processa os vídeos quadro a quadro com `model.track()`. A contagem principal considera carros, motocicletas, ônibus e caminhões.
3. O código compara a posição atual do centro da caixa com a última posição registrada para o mesmo ID. Cada ID é contado uma vez por vídeo quando esse deslocamento intersecta a linha virtual.
4. Os vídeos de saída exibem as caixas, os IDs, os centros, a linha virtual e o contador acumulado. O FFmpeg converte as saídas para H.264 para reprodução no navegador.
5. Os experimentos extras incluem uma configuração com ByteTrack e um YOLO11n ajustado ao VisDrone.

## Como executar no Colab

1. Baixe o [notebook do projeto](Desafio_semana_4-Grup7.ipynb), importe-o no Google Colab e selecione um ambiente com GPU. Confirme que a sessão tem acesso à internet e ao FFmpeg.
2. Execute as células de instalação e download dos vídeos. Se `kagglehub` estiver ausente, execute `%pip install kagglehub`. Confira se `Cam_v1` a `Cam_v4` correspondem aos caminhos listados pelo download, pois as versões dos datasets podem mudar.
3. Execute as células do fluxo principal em ordem. Os vídeos anotados são gravados em `videos_resultado/`, e a célula de processamento imprime a contagem final por vídeo. As prévias em H.264 usam os nomes `resultado_v1_web.mp4` a `resultado_v4_web.mp4`.
4. Para comparar com ByteTrack, inicialize uma instância nova com `YOLO("yolo11m.pt")` antes do bloco extra. Na cópia atual, `persist=True` pode reaproveitar o tracker anterior e carregar estado de um vídeo para o seguinte. Passar `persist=False` nas chamadas que recebem o caminho de um vídeo completo permite reiniciar o tracker ao mudar de vídeo. [Referência do Ultralytics sobre persistência](https://docs.ultralytics.com/reference/trackers/track/)
5. Para executar o teste VisDrone, envie o checkpoint `best (1).pt` para `/content`. Esse arquivo é necessário apenas para o experimento opcional. Baixe as prévias que quiser guardar antes de executar outro bloco, pois os mesmos nomes de saída são reutilizados.

## Resultados

| Experimento | Observação registrada no notebook |
| --- | --- |
| YOLO11m | A contagem pareceu coerente visualmente na maior parte dos vídeos. O vídeo 2 apresentou dificuldade pelo ângulo da câmera e pela posição da linha. Houve confusão de classe entre carro e caminhão, sobretudo com vans. |
| ByteTrack | A análise relata IDs visualmente mais estáveis. A comparação precisa de nova execução com um tracker independente para confirmar essa observação. |
| YOLO11n ajustado ao VisDrone | A análise relata instabilidade no rastreamento. |

O notebook disponível contém as observações da equipe. As contagens, os vídeos anotados e as métricas de precisão precisam ser verificados em uma execução.

## Tecnologias

Python, Ultralytics YOLO, OpenCV, PyTorch, KaggleHub, ByteTrack e FFmpeg.

## Equipe

Ana Carolina Dutra, Camila Gois de Jesus e Guilherme Magalhães Junior.  
**Professor** — Matheus Vanzan.
