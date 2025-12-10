# Algoritmo de Shor — Computação Quântica

<div align="center">

![Quantum Computing](https://img.shields.io/badge/Quantum-Computing-blueviolet?style=for-the-badge)
![Qiskit](https://img.shields.io/badge/Qiskit-Latest-6929C4?style=for-the-badge&logo=ibm)
![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**Implementação educacional do Algoritmo de Shor para fatoração de números usando computação quântica**

[Sobre](#-sobre) • [Estrutura](#-estrutura-do-projeto) • [Tecnologias](#-tecnologias) • [Início Rápido](#-início-rápido) • [Conteúdo](#-conteúdo) • [Resultados](#-resultados)

</div>

---

## 📋 Sobre

Este repositório contém uma implementação educacional completa do **Algoritmo de Shor**, um dos algoritmos quânticos mais importantes que demonstra a supremacia computacional de computadores quânticos sobre clássicos para o problema de fatoração de números inteiros.

O algoritmo de Shor é capaz de fatorar números inteiros em tempo **polinomial** (O((log N)³)), enquanto os melhores algoritmos clássicos conhecidos operam em tempo **subexponencial**. Esta característica torna o algoritmo particularmente relevante para criptografia, pois ameaça a segurança do sistema RSA.

### 🎯 Objetivos

- ✅ Compreender o funcionamento teórico do Algoritmo de Shor
- ✅ Implementar o algoritmo usando Qiskit (IBM Quantum)
- ✅ Executar em simuladores e hardware quântico real
- ✅ Explorar as limitações e desafios de escalabilidade
- ✅ Aplicar técnicas de mitigação de erro em hardware NISQ

---

## 📁 Estrutura do Projeto

```
Shor-Quantum_Computing/
├── README.md                    # Este arquivo
├── src/
│   ├── README.md               # Documentação detalhada do tutorial
│   ├── shors-algorithm.ipynb   # Tutorial principal: Fatorar N=15
│   ├── challenge.ipynb         # Desafio: Fatorar N=21
│   ├── challenge.md            # Relatório do desafio
│   └── ClassAboutShor.pdf      # Slides de referência teórica
└── .env                        # Credenciais IBM Quantum (não versionado)
```

---

## 🛠️ Tecnologias

| Tecnologia | Versão | Descrição |
|------------|--------|-----------|
| **Python** | 3.8+ | Linguagem de programação principal |
| **Qiskit** | Latest | Framework de computação quântica da IBM |
| **Qiskit IBM Runtime** | Latest | Execução em hardware quântico real |
| **NumPy** | Latest | Operações matemáticas e matrizes |
| **Matplotlib** | Latest | Visualização de circuitos e resultados |
| **Jupyter** | Latest | Ambiente interativo de notebooks |

---

## 🚀 Início Rápido

### Pré-requisitos

- Python 3.8 ou superior
- Jupyter Notebook ou JupyterLab
- Conta IBM Quantum (opcional, para execução em hardware real)

### Instalação

1. **Clone o repositório**
```bash
git clone https://github.com/MarceloAssis123/Shor-Quantum_Computing.git
cd Shor-Quantum_Computing
```

2. **Instale as dependências**
```bash
pip install qiskit qiskit-ibm-runtime python-dotenv numpy pandas matplotlib jupyter
```

3. **Configure credenciais IBM Quantum** (opcional)

Crie um arquivo `.env` na raiz do projeto:
```env
IBM_API_KEY=seu_token_aqui
QISKIT_IBM_INSTANCE=
```

> 💡 **Dica:** Obtenha suas credenciais em [IBM Quantum Platform](https://quantum-computing.ibm.com/)

4. **Execute os notebooks**
```bash
jupyter notebook src/
```

---

## 📚 Conteúdo

### 1️⃣ Tutorial Principal: Fatorar N=15

**Arquivo:** [`src/shors-algorithm.ipynb`](src/shors-algorithm.ipynb)

Tutorial completo e detalhado que implementa o Algoritmo de Shor para fatorar **N = 15** usando **a = 2**.

#### 🔍 O que você vai aprender:

- **Fundamentos teóricos:**
  - Problema de fatoração e sua relação com RSA
  - Redução do problema de fatoração para determinação de ordem
  - Estimativa de Fase Quântica (QPE)
  - Transformada de Fourier Quântica (QFT)

- **Implementação prática:**
  - Construção manual de operadores de multiplicação modular (M₂, M₄ mod 15)
  - Circuito completo de QPE com 8 qubits de controle e 4 qubits alvo
  - Execução em simulador ideal vs. hardware real
  - Técnicas de mitigação de erro (desacoplamento dinâmico, gate twirling)

- **Pós-processamento clássico:**
  - Conversão de medições para estimativas de fase
  - Uso de frações contínuas para encontrar a ordem r
  - Cálculo dos fatores usando gcd(a^(r/2) ± 1, N)

#### 📊 Resultados esperados:
```
N = 15, a = 2
Ordem encontrada: r = 4
Fatores: {3, 5}
Verificação: 3 × 5 = 15 ✓
```

---

### 2️⃣ Desafio: Fatorar N=21

**Arquivos:** [`src/challenge.ipynb`](src/challenge.ipynb) | [`src/challenge.md`](src/challenge.md)

Desafio prático que estende o tutorial principal para fatorar **N = 21** usando **a = 2**.

#### 🎯 Parâmetros do problema:

| Parâmetro | Valor | Descrição |
|-----------|-------|-----------|
| **N** | 21 | Número a ser fatorado |
| **a** | 2 | Base coprima com N |
| **num_target** | 5 qubits | Representação de estados até N |
| **num_control** | 10 qubits | Precisão da QPE |
| **r (esperado)** | 6 | Ordem de 2 módulo 21 |
| **Fatores** | {3, 7} | 21 = 3 × 7 |

#### 🔧 Otimizações implementadas:

- **Síntese via permutações:** Uso de `perm2circuit` para reduzir profundidade do circuito
- **Parser robusto:** Tratamento de diferentes formatos de retorno do `SamplerV2`
- **Correção de endianness:** Inversão de bitstrings little-endian do Qiskit
- **Filtragem estatística:** Análise das medições mais frequentes

#### 📊 Resultados obtidos:

```
Backend: ibm_marrakesh
Shots: 1024
Fatores encontrados: {3, 7} ✅
Ordem identificada: r = 3, 4, 6, 8 (múltiplos/divisores)
```

---

## 📈 Resultados

### ✅ Sucessos Alcançados

| Item | Status | Descrição |
|------|--------|-----------|
| Fatoração N=15 | ✅ | Fatores {3, 5} recuperados com sucesso |
| Fatoração N=21 | ✅ | Fatores {3, 7} recuperados com sucesso |
| Execução em Hardware Real | ✅ | Testes em `ibm_marrakesh` com mitigação de erro |
| Otimização de Circuito | ✅ | Redução de profundidade via síntese de permutações |
| Mitigação de Ruído | ✅ | Desacoplamento dinâmico e gate twirling aplicados |

### ⚠️ Limitações e Desafios

- **Ruído quântico:** Hardware NISQ atual apresenta taxas de erro significativas
- **Escalabilidade limitada:** Circuitos profundos sofrem decoerência rapidamente
- **Parsing complexo:** API do Qiskit Runtime requer tratamento robusto de retornos
- **Endianness:** Necessidade de inversão de bitstrings (little-endian → big-endian)

### 🔮 Perspectivas Futuras

Para quebrar RSA-2048 bits, seriam necessários:

- **~20 milhões de qubits físicos** (incluindo correção de erros)
- **Profundidade de circuito ~10⁹ portas**
- **Taxas de erro <10⁻¹⁵** por porta
- **Técnicas avançadas de correção de erro quântico**

---

## 📖 Documentação Adicional

- **[Tutorial Detalhado](src/README.md):** Explicação completa do fluxo técnico e mapeamento slide→notebook
- **[Relatório do Desafio](src/challenge.md):** Análise detalhada dos resultados para N=21
- **[Slides de Referência](src/ClassAboutShor.pdf):** Material teórico base

---

## 🔗 Referências

1. **Shor, P.W.** (1994) - ["Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer"](https://arxiv.org/abs/quant-ph/9508027)
2. **IBM Quantum Documentation** - [Qiskit Tutorials](https://qiskit.org/documentation/)
3. **Nielsen & Chuang** - "Quantum Computation and Quantum Information" (2010)
4. **IBM Research** - [Shor's Algorithm on Eagle r3 Processor](https://research.ibm.com/blog/shor-quantum-computer)

---

## 👤 Autor

**Marcelo Assis**

- GitHub: [@MarceloAssis123](https://github.com/MarceloAssis123)
- Projeto: [Shor-Quantum_Computing](https://github.com/MarceloAssis123/Shor-Quantum_Computing)

---

## 📄 Licença

Este projeto é distribuído sob a licença MIT.

---

## 🤝 Contribuindo

Contribuições, issues e pull requests são bem-vindos! Sinta-se à vontade para:

- 🐛 Reportar bugs
- 💡 Sugerir novas funcionalidades
- 📝 Melhorar a documentação
- 🔧 Otimizar implementações

---

<div align="center">

**⭐ Se este projeto foi útil, considere dar uma estrela! ⭐**

*Desenvolvido com ❤️ para a comunidade de Computação Quântica*

</div>

