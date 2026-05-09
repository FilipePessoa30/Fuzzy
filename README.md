# Segmentação de Imagem com FCM e FICM (RGB / CIELAB / HSV)

## Resumo

Este repositório contém experimentos e implementações em Python de algoritmos de segmentação por agrupamento baseados em Fuzzy C-Means (FCM) e Fuzzy Intuitionistic C-Means (FICM) aplicados a espaços de cores RGB, CIELAB e HSV. O notebook principal executa segmentação, calcula gradientes de cor, detecta "áreas nebulosas" entre clusters e salva resultados (imagens e logs).

## Arquivos principais

- `Lógica_Fuzzy_Intuicionista_c_means_RGB_CIELAB_HSV.ipynb` - Notebook com todas as implementações e exemplos para RGB, CIELAB e HSV.

## Requisitos

- Python 3.8+
- numpy
- matplotlib
- scipy
- scikit-image
- scikit-learn
- opencv-python (cv2)
- pandas (opcional, usado em algumas rotinas de análise)

## Instalação rápida

Recomenda-se criar um ambiente virtual e instalar dependências:

pip install numpy matplotlib scipy scikit-image scikit-learn opencv-python pandas

## Uso

1. Abrir o notebook `Lógica_Fuzzy_Intuicionista_c_means_RGB_CIELAB_HSV.ipynb` (pode ser executado localmente ou em Colab). Há um botão "Open In Colab" no topo do notebook para execução direta no Google Colab.
2. Ajustar o caminho da pasta `folder_path` para a pasta com imagens a serem segmentadas (ex.: `/content/Teste` no Colab ou um caminho local).
3. Configurar parâmetros principais, por exemplo `cluster_sizes`, `distance_metrics` e `alpha_values` (para FICM).
4. Executar as células. O notebook gera:
   - Arquivos de texto com logs: `segmentacao_*_{image_name}_{c}_{distance_metric}.txt`
   - Imagens de resultado: `segmentacao_*_{image_name}_{c}_{distance_metric}.png`

## Observações sobre parâmetros

- `c` (número de clusters): definido em `cluster_sizes`.
- `distance_metric`: suportado `euclidean`, `mahalanobis`, `cosine`, `manhattan`.
- `m`: parâmetro de fuzzificação (padrão 2.0 nas funções).
- `alpha` (FICM): controla hesitação/intuição no modelo.

## Saída esperada

- Segmentação de imagem (mapa de clusters)
- Mapas de gradiente de cor
- Mapas de áreas nebulosas (valores normalizados 0..1)
- Logs com valores da função objetivo por iteração e métricas de variância

## Descrição do Projeto

## Objetivo

Este projeto tem por objetivo implementar, comparar e demonstrar técnicas fuzzy de segmentação de imagens aplicadas a cenas ambientais (por exemplo, imagens de incêndios, cobertura vegetal, cenas urbanas). A intenção é oferecer um conjunto de rotinas experimentais que permitam avaliar trade-offs entre qualidade de segmentação e custo computacional, facilitando escolha de parâmetros e representações de cor para diferentes requisitos de aplicação.

## Abordagem técnica

- Implementações: o notebook contém implementações do algoritmo Fuzzy C-Means (FCM) e de uma variante intuicionista (FICM) que incorpora um termo de "hesitação" controlado por `alpha`.
- Espaços de cor: as rotinas processam imagens em pelo menos três espaços de cor — RGB (direto), CIELAB (perceptualmente uniforme) e HSV (separação de tonalidade/intensidade) — permitindo comparar o efeito da representação cromática sobre a segmentação.
- Métricas e diagnósticos: o código registra a função objetivo por iteração, calcula variância dentro/fora de clusters, gera mapas de gradiente de cor e detecta "áreas nebulosas" (regiões de indecisão entre clusters) usando razões entre as menores distâncias aos centros.
- Distâncias: são suportadas múltiplas métricas de distância (`euclidean`, `mahalanobis`, `cosine`, `manhattan`), para estudar como a escolha da medida afeta convergência e qualidade.

## Estrutura do notebook

- Seções separadas para FCM em RGB, CIELAB e HSV.
- Seções para FICM (intuicionista) em diferentes espaços de cor, incluindo o cálculo do termo de hesitação e penalidade associada na função objetivo.
- Rotinas utilitárias para processar uma pasta de imagens, salvar logs e imagens de saída, e métodos auxiliares para estimar número de clusters (Elbow, AIC/BIC, Gap Statistic).

## Saídas e como interpretá-las

- Mapas de clusters: cada pixel recebe o rótulo do cluster dominante (argmax de `U`). Use-os para avaliação qualitativa.
- Mapas de áreas nebulosas: valores entre 0 e 1 que indicam pixels com associação ambígua — úteis para identificar fronteiras entre objetos e regiões de incerteza.
- Logs da função objetivo: ajudam a diagnosticar convergência e comparar execuções com parâmetros distintos.
- Métricas de variância: permitem avaliar o quão coesos são os clusters gerados.

## Pontos fortes e limitações

- Fortes: flexibilidade (múltiplos espaços de cor e métricas), análise de incerteza (áreas nebulosas), e suporte a variante intuicionista (FICM) para modelar hesitação.
- Limitações: implementações são voltadas para experimentação e podem não ser otimizadas para grandes imagens; métodos com matriz de covariância (Mahalanobis) podem ser custosos e instáveis sem regularização; não há pipeline automatizado de avaliação com ground-truth — isso deve ser adicionado para comparações quantitativas robustas.

## Sugestões para uso reprodutível

- Fixar `random_state` nas inicializações para reprodutibilidade.
- Registrar parâmetros e versões de dependências (por exemplo, um `requirements.txt`).
- Para avaliação quantitativa, fornecer masks de ground-truth e calcular IoU, precisão, recall e F1 entre segmentações e referência.

## Referência

- Sousa, F. P., Lanzillotti, R. S., Mendonça, A. M. P. de, Coelho, I. M., & Faria, C. O. de. (2025). Segmentação de imagens ambientais com algoritmos fuzzy: comparando precisão e eficiência. CONTRIBUCIONES A LAS CIENCIAS SOCIALES, 18(6), e18921. https://doi.org/10.55905/revconv.18n.6-282

### Como citar

Sousa, F. P., Lanzillotti, R. S., Mendonça, A. M. P. de, Coelho, I. M., & Faria, C. O. de (2025). Segmentação de imagens ambientais com algoritmos fuzzy: comparando precisão e eficiência. CONTRIBUCIONES A LAS CIENCIAS SOCIALES, 18(6), e18921. https://doi.org/10.55905/revconv.18n.6-282

### Resumo do artigo

O artigo apresenta uma comparação experimental entre algoritmos fuzzy aplicados à segmentação de imagens ambientais. Os autores implementam e avaliam variantes do Fuzzy C-Means (FCM), incluindo formulações intuicionistas (FICM), explorando diferentes espaços de cor (por exemplo, RGB, CIELAB e HSV) e opções de distância. A avaliação considera métricas de precisão de segmentação (como acurácia, precisão/recall e medidas de sobreposição tipo IoU) bem como métricas de eficiência computacional (tempo de execução e custo computacional), permitindo analisar trade-offs entre qualidade e desempenho.

O estudo descreve a metodologia experimental (configuração dos algoritmos, parâmetros testados e métricas adotadas) e discute resultados empíricos que orientam a seleção de métodos e parâmetros para aplicações práticas de monitoramento ambiental por imagem. A contribuição principal é a análise comparativa sistemática que ajuda pesquisadores e engenheiros a equilibrar requisitos de precisão e restrições de processamento.

### Link para o artigo

https://doi.org/10.55905/revconv.18n.6-282

## Licença e contato

Use conforme sua necessidade acadêmica ou experimental. Para dúvidas ou contribuições, abra uma issue no repositório.
