# Vagas para trauma eletivo

Calculadora de quantos pacientes de trauma eletivo a enfermaria pode receber na semana,
com leitura da lista da central de regulação, distribuição por dia e exportação em PDF.

Em uso: **https://hvmoderno.github.io/vagas-trauma-eletivo/**

Página única, sem dependências: `index.html` roda sozinho, direto do disco também.
Funciona offline, e nenhum dado sai do computador — não há servidor nem armazenamento.

## O cálculo

1. `operados nos últimos 7 dias − eletivos previstos nos próximos 7 dias − internados pelo
   ambulatório nos últimos 7 dias − pacientes em vaga zero nos últimos 7 dias` = saldo bruto.
2. O nível de contingência da emergência corta esse saldo, arredondando para baixo:

   | Emergência | Nível | Libera |
   |---|---|---|
   | até 28 pacientes | Rotina | 100% |
   | 29 a 33 | Grau 1 | 80% |
   | 34 a 42 | Grau 2 | 50% |
   | 43 ou mais | Grau 3 | não receber |

3. O percentual só incide sobre saldo positivo. Saldo zero ou negativo aparece como excedente.

## Fila da central

Cole a lista como ela chega (`número da regulação`, `fratura`, `cidade`, `dias`). A página
expande as abreviações conhecidas — `trans` e `colo` viram fraturas de fêmur, `tnz` vira
tornozelo, `lac` vira luxação acromioclavicular, `boa` vira Boa Viagem, `pedra` vira Pedra
Branca — e ordena por tempo de espera. Linhas não reconhecidas são contadas e avisadas,
nunca descartadas em silêncio.

Marque quem entra, escolha quantos pacientes por dia e copie o texto pronto, dia a dia,
para reenviar à central.

### Municípios

A referência são os 20 municípios da Macro Sertão Central: Aiuaba, Arneiroz, Banabuiú,
Boa Viagem, Canindé, Caridade, Choró, Ibaretama, Ibicuitinga, Itatira, Madalena, Milhã,
Parambu, Paramoti, Pedra Branca, Quixadá, Quixeramobim, Senador Pompeu, Solonópole e Tauá.
Chegam abreviados como `boa` (Boa Viagem), `pedra` (Pedra Branca) e `senador`
(Senador Pompeu), e a página devolve a grafia completa, com acento.

Cidade fora dessa relação é lida e contada normalmente, mas a página avisa quais são —
costuma ser erro de digitação, e um `quixeramobin` viraria uma fatia a mais no gráfico.

## Exportar PDF

Gera uma folha A4 com o perfil da demanda: números do cálculo, paciente mais antigo da
fila, pizza das cidades, tipos de fratura e histograma do tempo de espera. Os gráficos são
desenhados em SVG porque o navegador imprime sem fundos CSS por padrão.

A exportação só funciona com o arquivo aberto localmente. Em páginas embutidas em quadro
protegido, o navegador bloqueia a janela de impressão e a folha é apenas exibida na tela.
