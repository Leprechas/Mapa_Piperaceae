# Mapa de coletas do Herbário do Núpelia UEM (HNUP-UEM)
[Link para o site '-'](https://leprechas.github.io/Mapa_Piperaceae/)

Aplicação web aberta para **visualização, filtragem e análise** dos registros de coletas vinculados ao Herbário do Núcleo de Pesquisas em Limnologia, Ictiologia e Aquicultura da Universidade Estadual de Maringá (HNUP-UEM), com ênfase nas espécies estudadas pelo **Laboratório de Vegetação Ripária (Nupélia/UEM)**.  
> O exemplo atual pode estar focado em grupos específicos (ex.: **Piperaceae**), mas a aplicação é **genérica** e pode ser usada por outros herbários/labs com seus próprios dados.

---

## Motivação

Este projeto foi criado para:
- **Dar transparência e acesso público** às informações de coletas do herbário.
- **Apoiar pesquisa, ensino e extensão**, oferecendo uma visualização interativa, responsiva e de fácil uso.
- **Padronizar a consulta taxonômica** (Família → Gênero → Espécie) integrando **filtros ambientais** (Ano e Município).
- **Servir como modelo reprodutível** para que outros herbários/laboratórios adaptem a mesma solução com seus dados.

---

## Visão geral

- **Stack**: HTML/CSS/JavaScript (vanilla) + [Leaflet](https://leafletjs.com/), [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster) e [Chart.js](https://www.chartjs.org/).
- **Dados**: arquivo `dados1.json` (UTF-8) com as amostras.
- **Arquivos principais**:
  - `index.html` — estrutura da página e estilos.
  - `mapa_script.js` — lógica de carregamento, filtros, seleção taxonômica e gráfico.
  - `dados1.json` — registros de coletas.
  - `logo_uem.jpeg`, `logo_lab.png` — logotipos (personalizáveis).

---

## Funcionalidades

- **Lista taxonômica hierárquica** (Família → Gênero → Espécie) com contagem por nível.
- **Barra de busca por espécie** (digite o **epíteto específico**; pressionar *Enter* aplica a seleção).
- **Filtros por Ano e por Município**, utilizáveis isoladamente ou combinados com a seleção taxonômica.
- **Mapa interativo com *marker clustering*** (Leaflet + MarkerCluster) para alto volume de pontos.
- **Pop-ups detalhados** (espécie, gênero, família, ano, município, habitat, descrição, notas).
- **Gráfico dinâmico (Chart.js)** de **coletas por ano**, que:
  - exibe o **total** quando nada está selecionado;
  - mostra **séries empilhadas** por família/gênero/espécie quando houver seleção;
  - **respeita os filtros** de ano e município ativos.
- **Links rápidos para o Reflora (Lista Brasil)** a partir dos táxons selecionados.
- **Layout responsivo** e seção de **Informações relevantes**.

---

## Como o sistema funciona (arquitetura e fluxo)

1. **Carregamento dos dados**  
   `dados1.json` é lido via `fetch`. Cada registro válido gera um **marcador** no mapa.  
   → Registros **sem coordenadas** ou com `(0, 0)` são **ignorados**.

2. **Indexação taxonômica e contagens**  
   As espécies são agrupadas por **gênero** e os gêneros por **família**. Contadores são mantidos para rotular a lista hierárquica.

3. **Metadados para filtros**  
   Cada marcador recebe metacampos:
   - `m.__year` (ano de coleta);
   - `m.__municipio` (município).  
   Os filtros **operam sobre esses metadados**, e não por busca de texto no *popup* — solução mais robusta e eficiente.

4. **Seleção taxonômica + filtros ambientais**  
   - O usuário marca caixas em qualquer nível (família/gênero/espécie) e/ou ativa filtros por ano/município.  
   - O mapa exibe apenas marcadores que **pertencem à seleção** e **passam nos filtros**.  
   - O **gráfico** é recalculado para o subconjunto visível, **alinhando** os anos entre as séries (empilhamento coerente).

5. **Busca por espécie**  
   A busca pelo epíteto marca automaticamente a espécie correspondente, **aplica a seleção** e **respeita os filtros** ativos.

6. **Caixa “Mais informações”**  
   Gera links diretos para o **Reflora – Lista Brasil** com o táxon selecionado (espécies → “Gênero epíteto”; gêneros e famílias usam “Nome Completo”).

---

## Estrutura dos dados (`dados1.json`)

Cada item representa uma coleta (ou ponto representativo). Campos marcados com ★ são utilizados pela interface.

```json
[
  {
    "nome": "angustiflora",          // ★ epíteto específico (string)
    "genero": "Justicia",            // ★ (string)
    "familia": "Acanthaceae",        // ★ (string)
    "latitude": -22.3456,            // ★ (número decimal; WGS84)
    "longitude": -53.1234,           // ★ (número decimal; WGS84)
    "ano_coleta": 2014,              // ★ (inteiro; filtros e gráfico)
    "municipio": "Porto Rico",       // ★ (string; filtros)
    "numero_coletas": 1,             // opcional; informativo no popup
    "habitat": "Mata ciliar",        // opcional
    "descricao": "Arbusto ...",      // opcional
    "notas": "Observações livres"    // opcional
  }
]
```

## Créditos e contato

- **Laboratório de Vegetação Ripária – Nupélia/UEM**
- **Contato daresponsável pelo laboratório e herbário:** [kazue@nupelia.uem.br](mailto:kazue@nupelia.uem.br)
- **Herbário do Nupélia/UEM (HNUP)**
- **Referência taxonômica:** Reflora – Lista Brasil, etc.
- **Contato para informações do site:** [vitorbarelli@gmail.com](mailto:vitorbarelli@gmail.com)

## Citação sugerida

> Barelli, V. E. G.; Dalbianco, M. E. V.; Fernandes, C. E. B.; Romagnolo, M. B.; BARELLI, M. A. A.; Kawakite, K.(2025). *Mapa de coletas do Herbário do Núpelia UEM (HNUP-UEM) — aplicação web de visualização, filtros e análise temporal*. Repositório GitHub. Disponível em: **https://github.com/Leprechas/Mapa_Piperaceae**.
