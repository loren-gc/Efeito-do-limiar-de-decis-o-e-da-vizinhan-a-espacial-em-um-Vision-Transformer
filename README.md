# Rejeição de Espécies Não Vistas em Séries Fenológicas do Cerrado

Este repositório acompanha o artigo **"Rejeição de espécies não vistas em séries fenológicas do Cerrado: efeito do limiar de decisão e da vizinhança espacial em um Vision Transformer"**, desenvolvido no Departamento de Computação de Sorocaba (UFSCar).

## Sobre o Projeto

Classificadores treinados em conjunto fechado atribuem rótulos de espécies conhecidas a qualquer espécie nova encontrada em campo. Este trabalho avalia o reconhecimento de conjunto aberto (*open-set recognition*) aplicado a séries temporais de imagens fenológicas do Cerrado (conjunto **Itirapina**, com 6 espécies), utilizando um **Vision Transformer (ViT) com tokenização temporal** e cabeça de rejeição.

### Hipóteses Avaliadas
* **H1:** Reduzir o limiar de rejeição ($\tau$) de `0,5` para `0,4` aumenta o *H-score*.
* **H2:** Ampliar a vizinhança espacial de entrada de `4` para `8` pixels não aumenta o *H-score*.

## Metodologia

* **Conjunto de dados:** Itirapina (24.396 pixels rotulados em 6 espécies do Cerrado, divididas em 3 grupos funcionais fenológicos: decíduas, perenifólias e semidecíduas).
* **Modelo:** ViT temporal (1,59M parâmetros; 0,12 GFLOPs) com duas cabeças de classificação — uma de conjunto fechado ($G_c$) e uma binária ($G_0$) que estima a probabilidade de o pixel pertencer a uma classe desconhecida ($p_{desc} > \tau$).
* **Treinamento:** Otimizador AdamW ($\text{lr} = 3 \times 10^{-4}$), *batch size* 32 e 30 épocas.
* **Protocolos:** *Leave-one-out* por espécie (6 dobras) e análise complementar por grupos funcionais (3 dobras).

## Principais Resultados

| Configuração | Protocolo | Vizinhança | Limiar ($\tau$) | $Acc_{conh}$ (média) | $Acc_{desc}$ (média) | H-score (média) |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **A** | Leave-one-out | 4 | 0,4 | 0,489 | 0,062 | **0,108** |
| **B** | Leave-one-out | 4 | 0,5 | 0,517 | 0,039 | 0,071 |
| **C** | Leave-one-out | 8 | 0,5 | 0,467 | 0,036 | 0,065 |
| **D** | Grupos funcionais | 4 | 0,5 | 0,512 | 0,019 | 0,036 |

* **Efeito do limiar (H1):** A redução de $\tau$ de `0,5` para `0,4` elevou o *H-score* nas 6 espécies (diferença média de `+0,037`; IC95%: `0,013` a `0,061`), sem variação detectável na acurácia das classes conhecidas ($Acc_{conh}$).
* **Efeito da vizinhança (H2):** A vizinhança de 8 pixels não elevou o *H-score* (diferença média de `-0,007`) e reduziu $Acc_{conh}$ em 5 das 6 espécies.
* **Desempenho geral:** Em todas as configurações, $Acc_{desc}$ não ultrapassou `11,5%`, indicando que a maior parte dos pixels de espécies não vistas ainda foi classificada como espécie conhecida.

## Limitações
* Execução única por configuração (variância entre sementes de treinamento não medida).
* Avaliação restrita a 6 espécies de uma única campanha fenológica.
* Ausência de métricas independentes de limiar (como AUROC) e de *baselines* simples de rejeição.

## Autores
* **Anderson Cristiano Sassaki Gonçalves** — UFSCar (Campus Sorocaba)
* **Lorenzo Grippo Chiachio** — UFSCar (Campus Sorocaba)
