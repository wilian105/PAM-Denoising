🌊 PAM-Denoising: Filtro de Wiener

Repositório dedicado à implementação de um Filtro de Wiener para redução de ruído em sinais de Monitoramento Acústico Passivo (PAM). O código foi projetado para processamento em lote de sinais acústicos subaquáticos, automatizando o pareamento de dados, a filtragem e a extração de métricas de avaliação.

📋 Visão Geral

A implementação processa múltiplos arquivos de áudio ruidosos simultaneamente. O fluxo de trabalho inclui o pareamento automático com os respectivos sinais limpos de referência, a aplicação do filtro de Wiener com abordagem decision-directed, o cálculo de métricas de desempenho e a geração de espectrogramas comparativos para facilitar a análise qualitativa — ideal para rotinas de pré-processamento em análises LOFAR e DEMON.

🛠️ Tecnologias Utilizadas

O sistema foi desenvolvido em Python, suportado pelas seguintes bibliotecas:

librosa: Operações de transformação (STFT e ISTFT).

NumPy: Processamento de operações matriciais.

SciPy: Suavização do mapa de ganhos espectrais.

Pandas: Estruturação e organização das métricas em dados tabulares.

Matplotlib: Geração e visualização dos espectrogramas.

✨ Principais Funcionalidades

Pareamento Automático de Arquivos
O script associa cada sinal ruidoso ao seu respectivo sinal limpo através de convenções de nomenclatura. O algoritmo verifica correspondências diretas de radicais e substituições sistemáticas (ex: chuva por limpo, ruido por limpo). Como critério de reserva (fallback), utiliza o prefixo comum antes do primeiro separador, permitindo que a execução em lote ocorra sem intervenção manual.

Estimativa Dinâmica de Ruído
A duração do trecho inicial utilizado para estimar a densidade espectral do ruído é ajustada automaticamente. O sistema utiliza o menor valor entre 20 segundos e 1/4 da duração total do sinal, garantindo que a estimativa não ultrapasse a porção disponível em áudios mais curtos.

Reconstrução e Normalização
Após a filtragem, o sinal é reconstruído no domínio do tempo (ISTFT) e, quando necessário, normalizado pelo seu valor de pico. Os arquivos resultantes são salvos com o sufixo _wiener, mantendo a taxa de amostragem original.

⚙️ Metodologia de Filtragem

Para cada par de arquivos processado, a função de filtragem segue o pipeline:

Cálculo da Short-Time Fourier Transform (STFT).

Estimativa da densidade espectral de potência do ruído com base no trecho inicial isolado.

Aplicação da abordagem decision-directed, incorporando piso espectral e suavização para mitigar artefatos (ruído musical).

Reconstrução do sinal temporal via ISTFT.

📊 Resultados e Avaliação

Ao final do processamento em lote, o pipeline entrega um conjunto completo de avaliações:

Métricas Quantitativas: Os resultados de cada arquivo são consolidados em uma tabela do Pandas, apresentando também a média geral de desempenho.

Análise Qualitativa: Geração automática de espectrogramas comparativos lado a lado (Sinal Ruidoso vs. Filtrado por Wiener vs. Sinal Limpo).

Exportação Prática: Todos os sinais processados (_wiener), espectrogramas e tabelas de métricas são compactados em um único arquivo .zip para facilitar o armazenamento e a transferência dos resultados.


🌊 PAM-Denoising: Rede Neural Convolucional (CNN)

Repositório dedicado à implementação de uma Rede Neural Convolucional (CNN) para redução de ruído em sinais de Monitoramento Acústico Passivo (PAM). O código abrange o pipeline completo de deep learning: desde a geração de conjuntos de dados sintéticos até a arquitetura da rede, rotina de treinamento, reconstrução temporal do sinal e extração de métricas de avaliação.

📋 Visão Geral

A implementação foi projetada para processar múltiplos arquivos de áudio ruidosos em lote. O sistema realiza o pareamento automático com os respectivos sinais limpos de referência, calcula métricas de desempenho para validar o aprendizado do modelo e gera espectrogramas comparativos para análise qualitativa.

🛠️ Tecnologias e Ambiente

O sistema foi desenvolvido em Python e é perfeitamente compatível com o ambiente Google Colab, possuindo suporte nativo para aceleração por GPU. As principais bibliotecas utilizadas são:

TensorFlow (Keras): Construção da arquitetura da rede neural convolucional e gerenciamento da rotina de treinamento.

librosa: Operações de leitura, segmentação e manipulação de sinais de áudio.

NumPy: Processamento de arrays e operações matriciais complementares.

SciPy: Utilizada para a implementação de um filtro de Wiener espectral, servindo como base de comparação (baseline) para o desempenho da CNN.

🎧 Criação do Conjunto de Dados (Dataset)

Para garantir a robustez do modelo, o treinamento utiliza uma abordagem de síntese e aumento de dados em tempo de execução:

Sinais de Interesse: Gravações de motores de embarcações reais.

Fontes de Ruído: Gravações de ruído ambiente subaquático.

Superposição Controlada: Os pares de treinamento (sinal ruidoso vs. sinal limpo) são gerados inserindo trechos de ruído ambiente sobre os segmentos de motor.

Relação Sinal-Ruído (SNR): A intensidade do ruído inserido é sorteada aleatoriamente dentro de um intervalo de -12 dB a +15 dB, preparando a rede para operar em diversas condições adversas de SNR.

✨ Aplicação e Resultados

Validação em Cenário Real: O modelo treinado foi aplicado e validado com sucesso em um cenário prático utilizando a gravação de ruído irradiado de uma embarcação sob fortes condições de chuva (arquivos de teste: navio_chuva.wav e referência navio_limpo.wav).

Preservação das Características Iniciais: O sinal processado pela rede neural é reconstruído e salvo automaticamente com o sufixo _cnn, garantindo a preservação exata da taxa de amostragem original.

Análise Comparativa: O processamento em lote gera automaticamente os espectrogramas lado a lado e uma tabela de métricas, permitindo comparar diretamente a eficácia da CNN com os sinais originais e com a filtragem clássica.


