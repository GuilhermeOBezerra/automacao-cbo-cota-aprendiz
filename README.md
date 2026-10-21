# Automação de CBOs – Cota de Aprendizes (art. 429 da CLT)

Script em **Python + Selenium** que consulta cada CBO no site oficial do Ministério do Trabalho e identifica quais funções entram na base de cálculo da **cota de jovens aprendizes** (art. 429 da CLT).

## O problema

A área jurídica precisava saber quais funções da empresa entram na base de cálculo da cota de aprendizes. Para isso, cada CBO tinha que ser consultada manualmente no site do MTE, uma a uma, para cerca de **200 funcionários**.

Era um trabalho de **mais de 4 horas** de uma pessoa dedicada. Como concorria com outras demandas do RH, na prática a informação levava **semanas** para chegar a quem precisava decidir.

## O resultado

-  A consulta passou de **4h+ de trabalho manual para 7–10 minutos de execução**
-  A informação passou de **semanas** para **o mesmo dia**
-  Preenchimento padronizado direto na planilha, sem erro de digitação
-  Pode ser interrompido e retomado de onde parou

> **Sobre o captcha:** o site do MTE passou a exibir captcha recentemente. O script continua funcionando, mas pausa para que uma pessoa o resolva, então o tempo total agora depende de quantos captchas aparecem. O cache por família (abaixo) reduz bastante o número de consultas e, com isso, de captchas.

## Como funciona

1. Você seleciona a planilha `.xlsx` (com as colunas **CBO**, **TÍTULO** e **COTA**)
2. O script cria um **backup** automático (mantém os 5 mais recentes)
3. Abre o Chrome no site do MTE e consulta cada CBO
4. Preenche o **título da família ocupacional** e se ela tem a indicação do **art. 429 da CLT** (Sim/Não)
5. Salva a planilha **a cada linha**: se algo falhar, nada se perde

## Decisões de projeto

- **Cache por família de CBO:** analisando a estrutura da CBO, identifiquei que a indicação do art. 429 da CLT é atribuída no nível da **Família Ocupacional** (4 primeiros dígitos). O script consulta cada família apenas uma vez e reaproveita o resultado para todas as ocupações derivadas, o que reduz requisições e captchas e acelera a execução.
- **Captcha com humano no circuito:** o script não tenta burlar o captcha. Ele detecta, emite um aviso sonoro e espera a resolução (até 10 min).
- **Retomada automática:** começa na primeira linha com a coluna COTA vazia.
- **Normalização do código:** aceita a CBO com ou sem hífen (`252105` ou `2521-05`).
- **Aviso de planilha aberta:** se o arquivo estiver aberto no Excel, o script avisa em vez de travar.

## Como usar

**Pré-requisitos:** Python 3.10+ e Google Chrome instalados (Windows).

```bash
git clone https://github.com/GuilhermeOBezerra/automacao-cbo-cota-aprendiz.git
cd automacao-cbo-cota-aprendiz
pip install -r requirements.txt
python consulta_cbo.py
```

Selecione o arquivo `exemplo_cbos.xlsx` para testar.

> Em redes corporativas com proxy que exige login no navegador, altere `USAR_PROXY_MANUAL = True` no início do script.

## Estrutura

```
├── consulta_cbo.py      # script principal
├── exemplo_cbos.xlsx    # planilha de exemplo (dados fictícios)
├── requirements.txt     # dependências
└── README.md
```

## Tecnologias

Python · Selenium · openpyxl · Tkinter

## Observações

- Usa apenas dados públicos do site oficial da CBO (mtecbo.gov.br).
- A planilha de exemplo não contém dados de funcionários.
- O resultado apoia a análise da cota, mas a conferência final deve ser feita pelo RH/Jurídico.

---

Desenvolvido por **Guilherme Andrade** · [LinkedIn](https://www.linkedin.com/in/guilherme-andrade38)
