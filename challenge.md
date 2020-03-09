# Challenge 99

Diante do desafio proposto pela 99, meu objetivo foi criar, a partir de dados de viagens, métricas que ajudassem a entender a performance da empresa.

O processo foi realizado nas seguintes etapas:
1. Analise exploratória dos dados e preparação dos dados
2. Construção dos KPIs baseados nos dados disponíveis e alinhados com os objetivos e missão da empresa
3. Analise dos KPIs e conclusão dos dados

## 1. Análise Exploratória
   
   Realizei a analise da base diretamente no SQL para entender quais dados estão disponíveis
   
   Observação das tablelas
   
   ```sql
   select * 
         from orders  -- relacao de viagens solicitadas (ponto de partida/chegada), ID passageiro, forma de pgto
         limit 100;
   select * 
          from trips  -- historico de viagens solicitadas, forma de pagamento, valor pago, bandeira, ID passageiro e motorista
          limit 100;
   ```
 Observando as duas tabelas percebi que "orders" contém os mesmos dados que "trips", fiz apenas um teste para verificar que não existe diferença entre os destinos
 
 ```sql
 select
       orders.pickup_latitude  - trips .pickup_latitude    as lat_difer ,
       orders.pickup_longitude  - trips .pickup_longitude  as lng_difer
       from orders 
       join trips 
           on orders.order_id = trips.order_id 
           group by 1,2
           order by 1
```
Nesse caso, resolvi seguir a analise com a tabela *trips*

## 2. Construção dos KPIs e dashboard
   
 Missão :  Se tornar lider no mercado oferendo aos clientes menor preço, viagens mais rápidas e qualidade
    Grupo de  indicadores: Financeiro, Produtividade/Capacidade de atendimento e Qualidade
    
   **Financeiro**: O quano sustentável está o negócio
                
  **Produtividade/capacidade de atendimento** : Escalabilidade
   
   **Qualidade**:  
    
 Tabela de totais  - Essa query será usada para construir a consulta  do panorama geral de viagens Como Total de viagens, passageiros, motoristas, receita gerada total, % de receita por meio de pagamento, % de receita por tipo de corrida
 ```sql
select 
   date_trunc('day', pickup_datetime )::date       as data             ,
   count(order_id )                                as  orders    ,                                     
   count(distinct passenger_id )                   as  passagers        ,        
   count(distinct driver_id    )                   as  drivers    ,
   round(sum(total_amount )::integer,2)            as  total_amount     ,
   round(sum(fare_amount  )::integer,2)            as  fare_amount        
   from trips
   group by 1


```
 Tabela de corridas - Essa query utiliza a estrutura simplificada da tabela "trips" para criar consultas sobre tempo médio por viagem, preço por milhas, ticke médio, LTV, distribuição de corrida por horário 
 
 
   ```sql
select 
   order_id          as viagem_id       ,
    case when rate_code  =1 then 'Standard_rate'
        when rate_code  =2 then 'JFK'
        when rate_code  =3 then 'Newark'
        when rate_code  =4 then 'Nassau_westchester'
        when rate_code  =5 then 'Negotiated_fare'
        when rate_code  =6 then 'group_ride'
        end                                       as rate        ,
   case when payment_type ='CRD' then 'credit_card'
        when payment_type ='CSH' then  'cash'
        when payment_type ='DIS' then 'dispute'
        when payment_type ='UNK' then 'unknown'
        when payment_type ='NOC'then 'no_charge'
        end                                       as payment_type ,
   passenger_id      as passageiro      ,
   driver_id         as motorista       ,
   pickup_datetime   as viagem_inicio   ,
   dropoff_datetime  as viagem_fim      ,
   pickup_latitude   as inicio_latitude ,
   pickup_longitude  as inicio_longitude,  
   dropoff_latitude  as fim_latitude    ,
   dropoff_longitude as fim_longitude   ,
   trip_distance     as di_percorrida_mi,
   total_amount      as valor_total     ,
   fare_amount       as valor_viagem    
from trips 
limit 1000 
```

   Ferramenta: Optei por utilizar o Tableau, já que é a ferramenta de BI usada pela empresa além de possuir um ainterface "amigavel", facil conecção com banco de dados e de compartilhamento 
    
    
   
   
   
