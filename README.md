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
