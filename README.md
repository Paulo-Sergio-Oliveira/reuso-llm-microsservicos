# Avaliação Comparativa de Modelos de Linguagem na Geração de Arquiteturas de Microsserviços com Foco no Reuso de Componentes e Serviços

Repositório de artefatos, prompts, respostas coletadas e planilhas de análise do experimento comparativo desenvolvido na disciplina **QXD0068 - Reuso de Software** (Universidade Federal do Ceará — Campus Quixadá), sob orientação do Prof. Francisco Victor da Silva Pinheiro.

## Equipe
- Emilly Paiva Belo
- Enzo Hariel Ferreira Gaspar
- Natan de Sousa Lucena
- Paulo Sergio Vieira de Oliveira
- Victor Veras Martins

## Questões de Pesquisa
- **RQ1:** Quais mecanismos de reuso são sugeridos pelas LLMs nas arquiteturas geradas?
- **RQ2:** Em que medida os diferentes modelos identificam oportunidades adequadas de reutilização?
- **RQ3:** Quais diferenças são observadas entre os modelos quanto às soluções de reuso propostas?

## Estrutura do Repositório
- `01_dados_origem/`: Requisitos textuais e arquiteturas de referência de **Bookstore** e **PetClinic**, extraídos na íntegra do pacote de replicação de Pereira et al. (2025).
- `02_prompts/`: Prompts few-shot congelados utilizados de forma idêntica em todos os modelos (`few-shot-bookstore.md` e `few-shot-petclinic.md`).
- `03_respostas_llms/`: Respostas completas exportadas das 18 execuções (3 modelos × 2 sistemas × 3 execuções), separadas por modelo.
- `04_analise/`: Planilha compartilhada de registro, classificação manual independente e consolidação das métricas.

## Guia de Coleta de Dados (Etapa 2)

| Responsável | Modelo | Interface de Chat | Pasta de Destino |
| :--- | :--- | :--- | :--- |
| Victor Veras Martins | Claude Opus 5.5 (Anthropic) | claude.ai | `03_respostas_llms/claude_opus_5_5/` |
| Enzo Hariel Ferreira Gaspar | Gemini 4 Neon (Google) | Gemini | `03_respostas_llms/gemini_4_neon/` |
| Natan de Sousa Lucena | GPT-6 Astra (OpenAI) | ChatGPT | `03_respostas_llms/gpt_6_astra/` |

### Regras obrigatórias de execução
1. **Não altere os arquivos em `02_prompts/`.** O texto enviado deve ser idêntico em todas as execuções.
2. **Execute 3 vezes para Bookstore e 3 vezes para PetClinic** (6 execuções por modelo, 18 no total).
3. **Abra uma conversa nova (chat limpo) para cada execução**, sem histórico prévio nem mensagens adicionais antes ou depois do prompt.
4. **Cole o conteúdo integral** de `02_prompts/few-shot-bookstore.md` ou `02_prompts/few-shot-petclinic.md` na primeira mensagem.
5. **Salve a resposta completa** usando o modelo `03_respostas_llms/TEMPLATE_EXECUCAO.md` com o padrão de nomenclatura:
   - `<modelo>_bookstore_run1.md`, `<modelo>_bookstore_run2.md`, `<modelo>_bookstore_run3.md`
   - `<modelo>_petclinic_run1.md`, `<modelo>_petclinic_run2.md`, `<modelo>_petclinic_run3.md`
   *(Exemplos: `claude_bookstore_run1.md`, `gemini_petclinic_run2.md`, `gpt_bookstore_run3.md`)*
6. **Não envie os arquivos de `01_dados_origem/arquiteturas_referencia/` aos modelos.** Eles serão utilizados apenas na etapa de análise.

## Créditos e Licença dos Dados de Origem
Os requisitos textuais e as arquiteturas de referência em `01_dados_origem/` são originários do pacote de replicação de **Pereira et al. (2025)**, disponível no Figshare sob o DOI [`10.6084/m9.figshare.29974345.v4`](https://doi.org/10.6084/m9.figshare.29974345.v4), distribuído sob a licença **Creative Commons Attribution 4.0 International (CC BY 4.0)** (cópia incluída em `01_dados_origem/`).
