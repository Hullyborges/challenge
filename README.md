# challenge

Diante do desafio proposto pela 99, meu objetivo foi criar  

1. Analise exploratória dos dados e preparação dos dados
2. Construição dos KPIs baseados nos dados dispoíveis e alinhados com os objetivos e missão da empresa
3. Analise final e conclusão dos dados

## 1. Análise Exploratória
   
   Para observar os dados conectei o banco ao programa R. Essa análise exploratória pode ser vista neste link
   
## 2. Contrstução dos KPIs e dashboard
   
 Missão :  Se tornar lider no mercado oferendo aos clientes menor preço, viagens mais rápidas e qualidade
    Grupo de  indicadores: Financeiro, Produtividade/Capacidade de atendimento e Qualidade
    
   Financeiro: Acompanhar o crescimento da receita
   * Crescimento da receita
   * Ticket médio 
   * LTV 
                
   Produtividade/capacidade de atendimento 
    
   Ferramenta: Optei por utilizar o Tableau, já que é a ferramenta de BI usada pela empresa além de escalável  e de forma automática por meio da conexão com o banco de dados  
    
 Tabela de totais 
 ```sql
select 
    date_trunc('day', pickup_datetime )::date       as dia       ,
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
         end                                       as pay_type ,
   sum(order_id )                                  as total_orders    ,                                     
   count(distinct passenger_id )                   as passagers ,
   count(distinct driver_id    )                   as total_drivers  ,
  round(sum(total_amount )::integer,2)             as paid_amount  ,
   sum(fare_amount  )                              as paid_trip
    from trips
    group by 1,2,3
    ```
 
 
 Informações de passageiros
   ```sql
   select *
   from 
     (select distinct 
       passenger_id          as passageiro,
       count(order_id)       as viagens_realizadas,
       max(pickup_datetime ) as ultima_viagem,
       min(pickup_datetime ) as primeira_viagem,
       sum(total_amount)     as valor_total_pago,
       sum(fare_amount)      as valor_viagem
from trips
group by 1
)main
```
    
    
   
   
   
