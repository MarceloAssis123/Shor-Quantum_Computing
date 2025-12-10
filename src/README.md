# Algoritmo de Shor — guia de estudo e reprodução

Este repositório contém um tutorial executável em Jupyter [notebook](./shors-algorithm.ipynb) que implementa o algoritmo de Shor para fatorar \(N = 15\) com \(a = 2\), seguindo as etapas apresentadas no slide de referência [`ShorOpt (1).pdf`](./ClassAboutShor.pdf). Abaixo você encontra um resumo conceitual, o mapeamento entre o conteúdo teórico e o notebook, e instruções práticas para reproduzir os resultados em simulador ou hardware IBM Quantum.

## Visão geral (do slide)
- **Contexto e impacto:** Shor permite fatoração em tempo polinomial em \(\log N\), ameaçando esquemas baseados em RSA; demonstrações atuais em hardware IBM (Eagle r3) rodam \(N=15\) em segundos ([slide](./ClassAboutShor.pdf)).
- **Objetivos do material:** entender fatoração e RSA, ordem modular, QPE, ligação QPE→ordem, passo a passo quântico, parte clássica de reconstrução de fatores, exemplo \(N=15, a=2\) e execução em hardware.
- **Roteiro teórico:** motivação/RSA → redução a problema de ordem → QPE → circuitos de exponenciação modular → exemplo \(N=15\) → execução/hardware → pós-processamento clássico → limitações e escalabilidade.

## Pré-requisitos e configuração
1) Python com pacotes: `qiskit`, `qiskit-ibm-runtime`, `python-dotenv`, `numpy`, `pandas`, `matplotlib`. Instale com:
   ```bash
   pip install qiskit qiskit-ibm-runtime python-dotenv numpy pandas matplotlib
   ```
2) Credenciais IBM Quantum (para executar em hardware):
   - Crie um arquivo `.env` na raiz do repositório com:
     ```
     IBM_API_KEY=...
     QISKIT_IBM_INSTANCE=...
     ```
   - As variáveis são carregadas na primeira célula de código (`load_dotenv()`).
3) Abra e execute [notebook](./shors-algorithm.ipynb) em seu ambiente Jupyter preferido.

## Mapeamento slide → notebook
- **Motivação, fatoração e RSA** → seções de histórico e definição de ordem (`## Etapa 1`, células MD 6–14) que recapitulam \(\mathbb{Z}_N^*\), ordem modular e motivação do problema de período, alinhadas ao roteiro do slide.
- **QPE e ligação com ordem** → células MD 7–11 descrevem o circuito de estimativa de fase e sua aplicação ao operador \(M_a\) (multiplicação modular), correspondendo às seções de “Estimativa de fase quântica” e “Determinação de ordem” do slide.
- **Operadores de exponenciação modular** → células MD 15–35 e códigos `M2mod15`, `M4mod15`, `controlled_*` (cód. 19–35) mostram a construção manual via portas `SWAP`, refletindo a parte do slide sobre funções periódicas \(f(x)=a^x \bmod N\) e implementação de \(M_a^{2^k}\).
- **Exemplo \(N=15, a=2\)** → seção MD 17–28 e códigos 26–32 calculam \(b=a^{2^k}\bmod N\), constroem \(M_2\), \(M_4\) e suas versões controladas, como ilustrado no slide “Exemplo: Shor para \(N=15\)”.
- **Circuito completo de determinação de ordem** → código 42 define `num_target=4`, `num_control=8`, importa `QFT` e monta o circuito de QPE com módulos controlados; corresponde às etapas de “Parte quântica: QPE + modular exponentiation” do slide.
- **Execução e mitigação** → seção “Etapa 3” (MD 46–65) e códigos 62–66 executam o circuito via `Sampler` no backend `ibm_marrakesh`, com desacoplamento dinâmico e gate twirling, ligando ao trecho do slide sobre execução em hardware e ruído.
- **Pós-processamento clássico** → seções MD 67–71 e códigos 48–58, 72 realizam: leitura de bitstrings, conversão para fases, aproximação por frações contínuas (`Fraction(...).limit_denominator(15)`), checagem de \(r\), cálculo de `gcd(a^{r/2}±1, N)`; isto implementa a parte “Conectando de volta à fatoração” do slide.
- **Limitações e escala / desafio** → células MD 73–76 discutem trabalhos relacionados, escala (milhões de qubits para RSA 2048) e propõem fatorar 21; refletem a seção “Limitações de escala” do slide.

## Fluxo técnico do notebook
1) **Imports e serviço IBM** (cód. 4, 45): carrega Qiskit, runtime e credenciais via `.env`.
2) **Construção manual de \(M_2\) e \(M_4\)** (cód. 19–35): portas `SWAP` implementam multiplicação modular; versões controladas suportam QPE.
3) **Alternativa via matriz de permutação** (cód. 36–40): compara profundidade ao usar `UnitaryGate` sintetizado; conclui que a abordagem manual é mais rasa.
4) **Circuito de QPE para ordem** (cód. 42): define registradores de controle (8 qubits) e alvo (4 qubits), aplica exponenciação modular controlada e QFT inversa.
5) **Simulação ideal** (cód. 48–57): usa contagens esperadas `{00000000,01000000,10000000,11000000}`, converte para fases \(0, \tfrac{1}{4}, \tfrac{1}{2}, \tfrac{3}{4}\) e extrai \(r=4\) via frações contínuas.
6) **Execução em hardware** (cód. 62–66): executa no backend `ibm_marrakesh` com DD + twirling; filtra contagens acima de um limiar para reduzir ruído.
7) **Fatoração clássica** (cód. 72): laço que repete medições/estimativas até encontrar fatores de \(N=15\) com `gcd`.
8) **Desafio adicional** (cód. 77): esqueleto para implementar \(M_a \bmod 21\).

## Como reproduzir
1) **Configurar ambiente**: instale dependências e defina `.env` com `IBM_API_KEY` e `QISKIT_IBM_INSTANCE` (ou use somente simulador e ignore as células de hardware).
2) **Executar etapas sequenciais** no notebook:
   - Rode as células de construção dos operadores (`M2mod15`, `M4mod15`, controladas).
   - Monte o circuito de determinação de ordem (cód. 42) e visualize se desejar.
   - Execute a simulação ideal (cód. 48–57) para obter \(r=4\).
   - (Opcional) Execute em hardware IBM (cód. 62–66) se tiver credenciais e acesso ao backend.
   - Rode o pós-processamento clássico (cód. 67–72) para recuperar os fatores \(3\) e \(5\).

## Observações e limitações
- Conforme destacado no [slide](./ClassAboutShor.pdf), a quebra de RSA de 2048 bits ainda está fora do alcance do hardware NISQ: seriam necessários milhões de qubits físicos e profundidades gigantescas. O notebook ilustra um caso educativo de pequena escala.
- O uso de `limit_denominator(15)` reflete o fato de que \(r < N\); ajuste o limite se experimentar outros valores de \(N\).
- A precisão de 8 qubits de controle (dois por qubit alvo) é suficiente para \(N=15\); ao escalar, aumente `num_control = 2 * num_target` e considere otimizações de profundidade.

## Referências
- Slide-base: [`ShorOpt (1).pdf`](./ClassAboutShor.pdf).
- Notebook: [notebook](./shors-algorithm.ipynb) (implementação passo a passo).
- Leituras adicionais listadas na seção “Referências” do notebook (célula MD 78).

