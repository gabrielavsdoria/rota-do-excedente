# Rota do Excedente

**Demo:** https://gabrielavsdoria.github.io/rota-do-excedente/

> Comida sobrando de um lado e faltando do outro é, antes de tudo, um problema de **logística**.

Protótipo que agrupa restaurantes com excedente de comida em rotas até cozinhas solidárias, para um entregador fazer no **horário de vale** entre o almoço e o jantar (15h–18h).

## Por que existe

Ideia que desenvolvi para o Desafio IA do processo seletivo de estágio do iFood (out/2026) e depois transformei em protótipo funcional.

- **24,2%** dos lares brasileiros estavam em insegurança alimentar em 2024 ([IBGE](https://agenciadenoticias.ibge.gov.br/pt/agencia-noticias/44728-mais-de-dois-milhoes-de-lares-saem-da-inseguranca-alimentar-em-2024.html)).
- **28%** do desperdício de alimentos no mundo acontece em *food service*, isto é, restaurantes, padarias e similares ([PNUMA, Food Waste Index 2024](https://www.unep.org/pt-br/noticias-e-reportagens/comunicado-de-imprensa/mundo-joga-fora-mais-de-1-bilhao-de-refeicoes-por-dia)).
- Iniciativas de doação já existem. Minha tese: o recurso que falta às cozinhas solidárias é **transporte**. Uma plataforma de delivery tem entregadores na rua e, **por hipótese**, capacidade ociosa no vale.

## Como funciona

1. Até as 15h, cada restaurante informa "tenho excedente hoje" e quantos kg.
2. O algoritmo agrupa 2 a 8 restaurantes próximos numa rota até a cozinha solidária mais perto.
3. Um entregador faz a rota no vale e recebe por rota.
4. O painel mostra kg resgatados, km, custo por kg e se a rota se paga.

### Algoritmo

| Passo | Método |
|---|---|
| Atribuição | Cada restaurante vai para a cozinha mais próxima. |
| Agrupamento | *Sweep*: ordena os restaurantes por ângulo em volta da cozinha e fecha uma rota quando bate a capacidade (kg) ou o máximo de paradas. |
| Ordem das paradas | Vizinho mais próximo, refinado com *2-opt*. A rota termina na cozinha. |
| Distância | Haversine × 1,3 (fator de ruas). |
| Tempo | Deslocamento na velocidade média + minutos por parada; alerta se passar da janela do vale. |
| Critério para desistir | Se o **custo por kg** da rota passar do custo de a cozinha **comprar** o alimento, a rota não se paga. |

Tudo roda no navegador, num único `index.html` com [Leaflet](https://leafletjs.com/) e OpenStreetMap. Sem backend e sem dados reais.

## Premissas (todas hipóteses a validar)

| Premissa | Valor padrão |
|---|---|
| Capacidade por rota | 30 kg |
| Máx. de paradas por rota | 8 |
| Pagamento por rota | R$ 35 |
| Custo de a cozinha comprar o alimento | R$ 12/kg |
| Velocidade média | 18 km/h |
| Tempo por parada | 5 min |
| kg por refeição | 0,4 kg |

Todas são editáveis na página. Restaurantes e cozinhas do exemplo são **fictícios**.

## Piloto que eu proporia

- **4 semanas**, 1 região de São Paulo, 20 restaurantes (self-service e padarias), 2 cozinhas solidárias, 2 entregadores.
- **Manual primeiro:** formulário + planilha + este algoritmo montando as rotas.
- **KPIs:** kg resgatados por semana · % de restaurantes que informam 3x/semana ou mais · custo por kg · ganho por hora do entregador no vale.
- **Riscos:** segurança do alimento (a Lei 14.016/2020 protege o doador quando o alimento está próprio para consumo; protocolo: só o que não foi ao balcão, embalado e identificado), adesão dos restaurantes e custo da rota (é o que o piloto mede).

## Próximos passos

- [ ] Geocodificar endereços em vez de clicar no mapa.
- [ ] Usar rotas reais (OSRM) em vez de linha reta × fator.
- [ ] Fase 2 com IA: prever, a partir do histórico, quanto cada restaurante vai sobrar e sugerir quanto produzir. Prevenir vem antes de redistribuir.

## Como construí

Construído com IA a partir do case que apresentei: o Claude como agente de código, com a lógica, as premissas e os critérios definidos por mim.

---

Feito por [Gabriela Dória](https://github.com/gabrielavsdoria) · Licença [MIT](LICENSE)
