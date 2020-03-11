 # Challenge 

 ## Etapas

O Desafio será realizado nas seguintes etapas: 

 1. Conexão ao banco e exploração dos dados brutos
 2. Definição do objetivo, criação de perguntas-guias e criação das queries de consulta ao banco de dados
 4. Criação do Dashboad
 5. Análise das métricas e conclusão

## Ferramentas utilizadas

 1. SQL
 2. R
 3. Tableau

Para realizar este desafio escolhi tabalhar com SQL consultas e transformação dos dados e Tableau para visualização.
O programa R será utilizado para conectar ao banco e permitir que as tabelas geradas pelas queries estejam disponíveis para visualização aqui

## Conectar ao banco de dados

Instalar  RpostgreSQL 


```R
install.packages("RPostgreSQL")
install.packages("DBI")
```

    Warning message:
    "package 'RPostgreSQL' is in use and will not be installed"Warning message:
    "package 'DBI' is in use and will not be installed"

Entrar com as credenciais no banco de dados


```R
library("RPostgreSQL")
```


```R
#database connection

dsn_database = "challenge"           
dsn_hostname = "challenge-99.cdu4dk9fjcht.us-east-1.rds.amazonaws.com" 
dsn_port = "5432"                
dsn_uid = "candidate"       
dsn_pwd = "99rules"      
```


```R
# estabelecer conexao

tryCatch({
    drv <- dbDriver("PostgreSQL")
    print("Connecting to database")
    conn <- dbConnect(drv, 
                 dbname = dsn_database,
                 host = dsn_hostname, 
                 port = dsn_port,
                 user = dsn_uid, 
                 password = dsn_pwd)
    print("Connected!")
    },
    error=function(cond) {
            print("Unable to connect to database.")
    })
```

    [1] "Connecting to database"
    [1] "Connected!"
    

## Explorar tabelas do banco de dados

Agora que a conexão foi estabelecida, vou observar as variaveis de cada tabela
1.trips 
2.orders


```R
#observar as variaveis da tabela trips

dbGetQuery(conn, "SELECT * FROM trips limit 10")
```


<table>
<thead><tr><th scope=col>order_id</th><th scope=col>pickup_datetime</th><th scope=col>dropoff_datetime</th><th scope=col>trip_distance</th><th scope=col>pickup_longitude</th><th scope=col>pickup_latitude</th><th scope=col>rate_code</th><th scope=col>dropoff_longitude</th><th scope=col>dropoff_latitude</th><th scope=col>payment_type</th><th scope=col>fare_amount</th><th scope=col>tip_amount</th><th scope=col>total_amount</th><th scope=col>passenger_id</th><th scope=col>driver_id</th></tr></thead>
<tbody>
	<tr><td>14362473           </td><td>2014-04-20 10:42:57</td><td>2014-04-20 10:49:06</td><td>0.90               </td><td>-73.97484          </td><td>40.75942           </td><td>1                  </td><td>-73.98035          </td><td>40.76969           </td><td>CRD                </td><td> 6.0               </td><td>1.30               </td><td> 7.80              </td><td>5.234568e+15       </td><td>5.234568e+15       </td></tr>
	<tr><td>12034238           </td><td>2014-04-02 11:52:00</td><td>2014-04-02 12:08:00</td><td>1.98               </td><td>-73.99130          </td><td>40.75568           </td><td>1                  </td><td>-73.98834          </td><td>40.74209           </td><td>CRD                </td><td>11.5               </td><td>2.30               </td><td>14.30              </td><td>5.234568e+15       </td><td>5.234568e+15       </td></tr>
	<tr><td> 5458856           </td><td>2014-04-18 09:47:00</td><td>2014-04-18 10:00:00</td><td>2.57               </td><td>-73.97969          </td><td>40.78952           </td><td>1                  </td><td>-73.96326          </td><td>40.76593           </td><td>CRD                </td><td>11.5               </td><td>2.00               </td><td>14.00              </td><td>5.234568e+15       </td><td>5.234568e+15       </td></tr>
	<tr><td> 3287441           </td><td>2014-03-25 14:38:00</td><td>2014-03-25 15:00:00</td><td>4.44               </td><td>-73.97321          </td><td>40.75115           </td><td>1                  </td><td>-73.96723          </td><td>40.80206           </td><td>CRD                </td><td>17.5               </td><td>4.38               </td><td>22.38              </td><td>5.234568e+15       </td><td>5.234568e+15       </td></tr>
	<tr><td> 5248138           </td><td>2014-04-15 10:44:00</td><td>2014-04-15 11:08:00</td><td>4.22               </td><td>-73.95974          </td><td>40.76010           </td><td>1                  </td><td>-73.99899          </td><td>40.73639           </td><td>CRD                </td><td>18.5               </td><td>2.50               </td><td>21.50              </td><td>5.234568e+15       </td><td>5.234568e+15       </td></tr>
	<tr><td> 2548526           </td><td>2014-03-16 15:08:00</td><td>2014-03-16 15:17:00</td><td>1.73               </td><td>-73.99321          </td><td>40.72218           </td><td>1                  </td><td>-73.97922          </td><td>40.74430           </td><td>CRD                </td><td> 8.5               </td><td>2.12               </td><td>11.12              </td><td>5.234568e+15       </td><td>5.234568e+15       </td></tr>
	<tr><td>  734111           </td><td>2014-05-24 15:55:38</td><td>2014-05-24 16:03:46</td><td>0.80               </td><td>-73.98912          </td><td>40.72146           </td><td>1                  </td><td>-73.99887          </td><td>40.72288           </td><td>CRD                </td><td> 7.0               </td><td>1.00               </td><td> 8.50              </td><td>5.234568e+15       </td><td>5.234568e+15       </td></tr>
	<tr><td> 8476403           </td><td>2014-03-01 13:47:00</td><td>2014-03-01 13:50:00</td><td>0.60               </td><td>-73.97505          </td><td>40.76152           </td><td>1                  </td><td>-73.97314          </td><td>40.75557           </td><td>CSH                </td><td> 4.0               </td><td>0.00               </td><td> 4.50              </td><td>5.234568e+15       </td><td>5.234568e+15       </td></tr>
	<tr><td> 7599677           </td><td>2014-04-23 22:48:16</td><td>2014-04-23 22:57:43</td><td>2.60               </td><td>-73.97044          </td><td>40.75866           </td><td>1                  </td><td>-73.94467          </td><td>40.77953           </td><td>CRD                </td><td>10.0               </td><td>2.20               </td><td>13.20              </td><td>5.234568e+15       </td><td>5.234568e+15       </td></tr>
	<tr><td> 8367496           </td><td>2014-05-23 17:08:41</td><td>2014-05-23 17:23:41</td><td>1.80               </td><td>-73.98120          </td><td>40.77408           </td><td>1                  </td><td>-73.97829          </td><td>40.75397           </td><td>CSH                </td><td>11.0               </td><td>0.00               </td><td>12.50              </td><td>5.234568e+15       </td><td>5.234568e+15       </td></tr>
</tbody>
</table>




```R
# observar variaveis da tabela orders

dbGetQuery(conn, "SELECT * FROM orders limit 10")
```


<table>
<thead><tr><th scope=col>order_id</th><th scope=col>passenger_id</th><th scope=col>pickup_datetime</th><th scope=col>pickup_longitude</th><th scope=col>pickup_latitude</th><th scope=col>payment_type</th></tr></thead>
<tbody>
	<tr><td>5942353            </td><td>5.234568e+15       </td><td>2014-04-26 11:00:00</td><td>-73.96506          </td><td>40.76652           </td><td>CRD                </td></tr>
	<tr><td>5942354            </td><td>5.234568e+15       </td><td>2014-04-26 11:01:00</td><td>-73.99757          </td><td>40.73639           </td><td>CSH                </td></tr>
	<tr><td>5942355            </td><td>5.234568e+15       </td><td>2014-04-26 12:12:00</td><td>-73.98696          </td><td>40.75985           </td><td>CSH                </td></tr>
	<tr><td>5942356            </td><td>5.234568e+15       </td><td>2014-04-26 11:56:00</td><td>-73.97683          </td><td>40.73928           </td><td>CRD                </td></tr>
	<tr><td>5942357            </td><td>5.234568e+15       </td><td>2014-04-26 13:39:00</td><td>-73.96052          </td><td>40.79747           </td><td>CRD                </td></tr>
	<tr><td>5942358            </td><td>5.234568e+15       </td><td>2014-04-26 11:33:00</td><td>-73.98935          </td><td>40.75861           </td><td>CSH                </td></tr>
	<tr><td>5942359            </td><td>5.234568e+15       </td><td>2014-04-26 11:29:00</td><td>-73.97151          </td><td>40.75644           </td><td>CRD                </td></tr>
	<tr><td>5942360            </td><td>5.234568e+15       </td><td>2014-04-26 11:34:00</td><td>-73.97638          </td><td>40.73983           </td><td>CRD                </td></tr>
	<tr><td>5942361            </td><td>5.234568e+15       </td><td>2014-04-26 11:44:00</td><td>-73.97129          </td><td>40.74681           </td><td>CRD                </td></tr>
	<tr><td>5942362            </td><td>5.234568e+15       </td><td>2014-04-26 11:44:00</td><td>-73.98376          </td><td>40.73809           </td><td>CSH                </td></tr>
</tbody>
</table>




```R
# Estabelecer a relação entre as tabelas e o numero total de observações em cada uma 


dbGetQuery(conn,"select 
                        count(o.order_id)  as contagem_orders,
                        count(t.order_id)  as contagem_trips
                        from orders o 
                        left join  trips t on t.order_id=o.order_id")

```


<table>
<thead><tr><th scope=col>contagem_orders</th><th scope=col>contagem_trips</th></tr></thead>
<tbody>
	<tr><td>1.5e+07 </td><td>11456987</td></tr>
</tbody>
</table>



A relação é feita pelo order_id de cada tabela 

A tabela *orders* possui mais de 15M de observações enquanto a tabela *trips* possui 11M 
isso significa que na tabela orders está o número de todas as viagens solicitadas a trips possui o número de viagens realizadas


 ## Criação de métricas

Após realizar a conexão com o banco e entender as variáveis em cada tabela, se inicia o processo exploração 
e criação de indicadores

Para isso preciso partir dos objetivos estratégicos da empresa que é ser líder no mercado oferendo o menor preço e a melhor qualidade de serviço para o passageiro. 

Com base nos objetivos da empresa, os macros indicadores  que devem ser analisados são: performance, receita,operação/capacidade


Esse macros indicadores geram algumas perguntas a serem respondidas por meio dos dados

*Performance

 * Estamos apresentando crescimento no numero de corridas?
 * Quantos clientes temos ativos na base?
 * O cliente está fidelizado?
 * Estamos aumentando/diminuindo o numero de conversões ao longo do tempo?

*Receita

 * A receita está evoluindo?
 * Quais canais/produtos tem contribuido para aviação na receita?
 * Quanto cada corrida gera de valor em média?

*Operação/capacidade de atendimento

 * Consigo saber onde as viagens estão concentradas?
 * Quais são os horários de pico?
 * Em média, quanto tempo duram as viagens? Isso varia com o horário?
 * Qual o preço que meu cliente para por milhas percorridas?

 ## Métricas de performance



 1. Número de corridas  - volume demandado do serviço, esse indicador ajuda a entender se há crescimento no negócio
 
 2. Passageiros ativos  - Crescimento da base de clientes
 
 3. Fidelização - estratégia de fidelização ou de atração a depender de quantas vezes o passageiro utilizou o serviço
 
 4. Taxa de conversão  - Quão eficiente está sendo o serviço ( nesse caso não há dados do motivo da disitência das outras                                 corridas)
 
 


```R
# corridas diarias - quantidade total de corridas realizadas
# Essa query faz a contagem de order_id aberto por dias

dbGetQuery(conn," -- corridas por dia
                 select date_trunc('day', pickup_datetime )::date as  date,
                        count(order_id)  as corridas
                 from trips
                 group by 1
                  limit 10        ")



# Passageiros ativos - contagem de clientes unicos  que realizaram uma corrida no mês
# contagem distinta do passeger_id aberto por dia 

dbGetQuery(conn," -- passageiros ativos por dia
                  select date_trunc('day',pickup_datetime )::date as date ,
                         count(distinct passenger_id ) as passageiros
                   from trips t 
                   group by 1
                   limit 10          ") 



# Fidelizacao -  Quantidade de corridas realizadas por cliente
# primiero realizo a contagem de order_id por passager_id para saber o numero de corridas pelo passager_id
#depois trabalho com o case when para definir o bin que gostaria de observar e finalmente a contagem de passager_id 

dbGetQuery(conn," -- contagem de passageiros por quantidade de corridas
                select 
                        case when viagens between 1 and 2 then       ' 1 a 2'
                             when viagens between 3 and 5 then       ' 3 a 5'
                             when viagens between 6 and 10 then      ' 6 a 10'
                             when viagens between 11 and 20 then     ' 11 a 20'
                             when viagens between 21 and 30 then     ' 21 a 30'
                             when viagens between 31 and 40 then     ' 31 a 40'
                             when viagens between 41 and 50 then     ' 41 a 50'
                             when viagens between 51 and 80 then     ' 51 a 80'
                             when viagens  between 81 and 100 then   '81 a 100'
                             when viagens  between  101 and 120 then '101 a 120'
                             when viagens  between  121 and 150 then '121 a 150'
                             when viagens  between  151 and 180 then '151 a 180'
                             when viagens  >180 then 'mais180' 
                                                     end  as faixa, 
                            count (a.passenger_id) passageiros
              from
                 (     select
                            distinct  passenger_id ,
                            count (order_id) as viagens
                      from trips
                      group by 1
                            )a
             group by 1
             limit 10          ")



# Taxa de conversão - clientes que realizaram a corrida / total de clientes solicitando corrida
 # Left join na tabela trips para somar todos os passagerios que solicitaram viagem( mesmo que não tenha realizado)
 # em seguida contagem de order_id na tabela orders e contagem na tabela trips

dbGetQuery(conn," -- contagem de viagens solicitadas / contagem de viagens realizadas
                select 
                       date_trunc('day',o.pickup_datetime)::date as date ,
                       count(o.order_id ) as viagens_solicitadas,
                       count(t.order_id ) as viagens_realizadas,
                       count(t.order_id )::numeric /count(o.order_id )::numeric as taxa_conversao
                 from 
                      orders o 
                left join trips  t on t.order_id =o.order_id 
                group by 1
                  limit 10 ")
           

```


<table>
<thead><tr><th scope=col>date</th><th scope=col>corridas</th></tr></thead>
<tbody>
	<tr><td>2014-03-01</td><td>211548    </td></tr>
	<tr><td>2014-03-02</td><td>176559    </td></tr>
	<tr><td>2014-03-03</td><td>177636    </td></tr>
	<tr><td>2014-03-04</td><td>190532    </td></tr>
	<tr><td>2014-03-05</td><td>194010    </td></tr>
	<tr><td>2014-03-06</td><td>202838    </td></tr>
	<tr><td>2014-03-07</td><td>209024    </td></tr>
	<tr><td>2014-03-08</td><td>210517    </td></tr>
	<tr><td>2014-03-09</td><td>180526    </td></tr>
	<tr><td>2014-03-10</td><td>168515    </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>date</th><th scope=col>passageiros</th></tr></thead>
<tbody>
	<tr><td>2014-03-01</td><td>148794    </td></tr>
	<tr><td>2014-03-02</td><td>129229    </td></tr>
	<tr><td>2014-03-03</td><td>129678    </td></tr>
	<tr><td>2014-03-04</td><td>137312    </td></tr>
	<tr><td>2014-03-05</td><td>139129    </td></tr>
	<tr><td>2014-03-06</td><td>143949    </td></tr>
	<tr><td>2014-03-07</td><td>147816    </td></tr>
	<tr><td>2014-03-08</td><td>148341    </td></tr>
	<tr><td>2014-03-09</td><td>131494    </td></tr>
	<tr><td>2014-03-10</td><td>124660    </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>faixa</th><th scope=col>passageiros</th></tr></thead>
<tbody>
	<tr><td>81 a 100 </td><td>   4325  </td></tr>
	<tr><td>151 a 180</td><td>     34  </td></tr>
	<tr><td>121 a 150</td><td>  14953  </td></tr>
	<tr><td> 1 a 2   </td><td>4384757  </td></tr>
	<tr><td> 6 a 10  </td><td>    115  </td></tr>
	<tr><td>101 a 120</td><td>  30669  </td></tr>
	<tr><td> 51 a 80 </td><td>     19  </td></tr>
	<tr><td> 3 a 5   </td><td> 152052  </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>date</th><th scope=col>viagens_solicitadas</th><th scope=col>viagens_realizadas</th><th scope=col>taxa_conversao</th></tr></thead>
<tbody>
	<tr><td>2014-03-01</td><td>277284    </td><td>211548    </td><td>0.7629290 </td></tr>
	<tr><td>2014-03-02</td><td>231264    </td><td>176559    </td><td>0.7634522 </td></tr>
	<tr><td>2014-03-03</td><td>232098    </td><td>177636    </td><td>0.7653491 </td></tr>
	<tr><td>2014-03-04</td><td>249604    </td><td>190532    </td><td>0.7633371 </td></tr>
	<tr><td>2014-03-05</td><td>254288    </td><td>194010    </td><td>0.7629538 </td></tr>
	<tr><td>2014-03-06</td><td>265404    </td><td>202838    </td><td>0.7642613 </td></tr>
	<tr><td>2014-03-07</td><td>273812    </td><td>209024    </td><td>0.7633851 </td></tr>
	<tr><td>2014-03-08</td><td>275816    </td><td>210517    </td><td>0.7632516 </td></tr>
	<tr><td>2014-03-09</td><td>236618    </td><td>180526    </td><td>0.7629428 </td></tr>
	<tr><td>2014-03-10</td><td>220990    </td><td>168515    </td><td>0.7625458 </td></tr>
</tbody>
</table>



 ## Métricas de receita

As métricas de receita ajudaram a entender a sustentabilidade do negócio.

 1. Receita total - Avaliar a sustentabilidade financeira
 
 2. Receita por forma de pagamento  - Qual o percentual de dependencia que a receita tem em relação a um meio de pagamento
 
 3. Receita por bandeira -  Qual gera maior tkt médio
 
 4. Ticket médio  - Valor gerado por cada passageiro/viagem. Se o ticket médio estiver baixo, precisará se manter o numero alto                     de corridas para garantir uma boa receita


```R
#receita total
 # soma do valor pago, excluindo o valor de gorjeta 

dbGetQuery(conn," --soma de receita por dia
                 select date_trunc('day', pickup_datetime )::date as  date,
                        sum(fare_amount) as receita
                 from  trips 
                 group by 1
limit 10")



# receita por forma de pagamento
 # primeiro criei uma tabela auxiliar para somar o valor de receita total
 # depois criei a query que calcula a receita aberta por mês e tipo de pagamento
 #  é feito um join por data com a tabela auxiliar para poder calcular o percentual do total de cada forma de pagamento

dbGetQuery(conn," -- soma da receita por forma de pagamento
                 with receita as (
                                   select date_trunc( 'month',pickup_datetime )::date date,
                                          round(sum(fare_amount)::integer,2) as receita
                                   from trips t 
                                   group by 1
 )
  select 
        date_trunc('month', pickup_datetime)::date  as  date,
        payment_type,
        r.receita as receita_total,
        round(sum(fare_amount)::integer,2)  as receita,
        round((round(sum(fare_amount)::integer,2) / r.receita),5) as percuntal_receita_mes
 from  trips t 
 join receita r  on r.date = date_trunc('month', t.pickup_datetime)::date
 group by 1,2,3
limit 10")


# receita por bandeira
 # mesmo que o anterior, subistintuindo a coluna payment_type por rate_code

dbGetQuery(conn," --soma da receita por bandeira
                with receita as (
                                   select date_trunc( 'month',pickup_datetime )::date date,
                                          round(sum(fare_amount)::integer,2) as receita
                                   from trips t 
                                   group by 1
 )
  select 
        date_trunc('month', pickup_datetime)::date  as  date,
        rate_code,
        r.receita as receita_total,
        round(sum(fare_amount)::integer,2)  as receita,
        round((round(sum(fare_amount)::integer,2) / r.receita),5) as percuntal_receita_mes
 from  trips t 
 join receita r  on r.date = date_trunc('month', t.pickup_datetime)::date
 group by 1,2,3
limit 10")



# Ticket médio por corridas - passageiros
   #soma dad receita dividido pela contagem distinta de passageiros e contagem de order_id

dbGetQuery(conn," -- soma da receita / contagem unica de passageiros
                 select date_trunc('month', pickup_datetime)::date as  date,
                        round(sum(fare_amount)::integer,2) as receita,
                        count(order_id) corridas,
                        count(distinct passenger_id) ,
                        round(sum(fare_amount)::integer,2) / count(order_id) as tkt_corridas,
                        round(sum(fare_amount)::integer,2) / count(distinct passenger_id) as tkt_cliente
                from trips 
                group by 1
limit 10")

```


<table>
<thead><tr><th scope=col>date</th><th scope=col>receita</th></tr></thead>
<tbody>
	<tr><td>2014-03-01</td><td>2508216   </td></tr>
	<tr><td>2014-03-02</td><td>2166266   </td></tr>
	<tr><td>2014-03-03</td><td>2004982   </td></tr>
	<tr><td>2014-03-04</td><td>2248707   </td></tr>
	<tr><td>2014-03-05</td><td>2349331   </td></tr>
	<tr><td>2014-03-06</td><td>2499741   </td></tr>
	<tr><td>2014-03-07</td><td>2573873   </td></tr>
	<tr><td>2014-03-08</td><td>2519669   </td></tr>
	<tr><td>2014-03-09</td><td>2271928   </td></tr>
	<tr><td>2014-03-10</td><td>2069117   </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>date</th><th scope=col>payment_type</th><th scope=col>receita_total</th><th scope=col>receita</th><th scope=col>percuntal_receita_mes</th></tr></thead>
<tbody>
	<tr><td>2014-03-01</td><td>CRD       </td><td>57323444  </td><td>35245142  </td><td>0.61485   </td></tr>
	<tr><td>2014-03-01</td><td>CSH       </td><td>57323444  </td><td>21608713  </td><td>0.37696   </td></tr>
	<tr><td>2014-03-01</td><td>DIS       </td><td>57323444  </td><td>   32932  </td><td>0.00057   </td></tr>
	<tr><td>2014-03-01</td><td>NOC       </td><td>57323444  </td><td>   97618  </td><td>0.00170   </td></tr>
	<tr><td>2014-03-01</td><td>UNK       </td><td>57323444  </td><td>  339039  </td><td>0.00591   </td></tr>
	<tr><td>2014-04-01</td><td>CRD       </td><td>69684834  </td><td>42184330  </td><td>0.60536   </td></tr>
	<tr><td>2014-04-01</td><td>CSH       </td><td>69684834  </td><td>26955080  </td><td>0.38681   </td></tr>
	<tr><td>2014-04-01</td><td>DIS       </td><td>69684834  </td><td>   56987  </td><td>0.00082   </td></tr>
	<tr><td>2014-04-01</td><td>NOC       </td><td>69684834  </td><td>  153080  </td><td>0.00220   </td></tr>
	<tr><td>2014-04-01</td><td>UNK       </td><td>69684834  </td><td>  335356  </td><td>0.00481   </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>date</th><th scope=col>rate_code</th><th scope=col>receita_total</th><th scope=col>receita</th><th scope=col>percuntal_receita_mes</th></tr></thead>
<tbody>
	<tr><td>2014-03-01</td><td>  0       </td><td>57323444  </td><td>    8711  </td><td>0.00015   </td></tr>
	<tr><td>2014-03-01</td><td>  1       </td><td>57323444  </td><td>51751613  </td><td>0.90280   </td></tr>
	<tr><td>2014-03-01</td><td>  2       </td><td>57323444  </td><td> 4245695  </td><td>0.07407   </td></tr>
	<tr><td>2014-03-01</td><td>  3       </td><td>57323444  </td><td>  439946  </td><td>0.00767   </td></tr>
	<tr><td>2014-03-01</td><td>  4       </td><td>57323444  </td><td>   83680  </td><td>0.00146   </td></tr>
	<tr><td>2014-03-01</td><td>  5       </td><td>57323444  </td><td>  793309  </td><td>0.01384   </td></tr>
	<tr><td>2014-03-01</td><td>  6       </td><td>57323444  </td><td>     210  </td><td>0.00000   </td></tr>
	<tr><td>2014-03-01</td><td>  8       </td><td>57323444  </td><td>      54  </td><td>0.00000   </td></tr>
	<tr><td>2014-03-01</td><td> 65       </td><td>57323444  </td><td>      23  </td><td>0.00000   </td></tr>
	<tr><td>2014-03-01</td><td>210       </td><td>57323444  </td><td>     203  </td><td>0.00000   </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>date</th><th scope=col>receita</th><th scope=col>corridas</th><th scope=col>count</th><th scope=col>tkt_corridas</th><th scope=col>tkt_cliente</th></tr></thead>
<tbody>
	<tr><td>2014-03-01</td><td>57323444  </td><td>4686895   </td><td>2170979   </td><td>12.23058  </td><td>26.40442  </td></tr>
	<tr><td>2014-04-01</td><td>69684834  </td><td>5583323   </td><td>2529867   </td><td>12.48089  </td><td>27.54486  </td></tr>
	<tr><td>2014-05-01</td><td>15364905  </td><td>1186769   </td><td> 626489   </td><td>12.94684  </td><td>24.52542  </td></tr>
</tbody>
</table>



## Métricas de Operação


 1. Preço por milha  - Monitorar picos de demanda e em que momento corre-se o risco de converter menos ou mais dado o valor da                        corrida 
 
 2. Volume de viagens por período  - Em quais momentos a operação sofre maior demanda e também maior oportunidade de ganhos 
 
 3. Demanda por geolocalização   - Onde há necessidade de alocar mais motoristas para atender de forma mais rápida os                                              passageiros


```R

# preço por milhas
  # soma do fare_amount dividido pela soma de trip_distance

dbGetQuery(conn,"  -- preco por milha
                    -- soma da receita / soma da distancia 
                 select 
                      date,
                      rate_code ,
                     (receita/distancia) r_d
                 from (
                        select 
                              date_trunc('day', pickup_datetime)::date date,
                              rate_code ,
                              round(sum(fare_amount)::integer,2) receita,
                              sum(trip_distance ) distancia,
                              count(passenger_id ) passageiros
                        from trips 
                        group by 1,2
                     )a
limit 10")



# intervalo de tempo por viagem
 # primeiro calculei a diferença em minutos entre dropoff_datetime e pickup_datetime
 # depois criei um bin (tamanho 10) com o valor das diferenças e fiz a contagem de order_id para cada bin

dbGetQuery(conn,"  -- intervalo de tempo por viagem
                   -- diferenca entre dropoff_datetime e pickup_datetime
                select date, 
                         case when time <=10 then '10'
                              when time <=20 then '20'
                              when time <=30 then '30'
                              when time <=40 then '40'
                              when time <=50 then '50'
                              when time <=60 then '60'
                              when time >=61 then '+1hour'
                                                     end as faixa,
                         count(order_id) corridas
                 from 
                        (
                        select 
                              date_trunc('hour',pickup_datetime)::date date,
                              order_id ,
                             ( DATE_PART('hour', dropoff_datetime::timestamp - pickup_datetime::timestamp) * 60 +
                               DATE_PART('minute', dropoff_datetime::timestamp - pickup_datetime::timestamp) )as time
                       from trips 
                        )a
                group by 1,2
limit 10")



# geolocalização - mapa criado diretamnte no Tableau com as variáveis de geolocalização dos clientes
 # criei um union all para conseguir distinguir pontos geograficos  de origem e de destino 

dbGetQuery(conn," -- geolocalizacao
                  select * ,
                         case when ponto ='origem' then 1 
                              else 2 end as ordem
                      from (
                               select o.order_id  order_id,
                                     'origem' as ponto,
                                      o.pickup_latitude  as latitude,
                                      o.pickup_longitude as longitude
                                from orders o
                                left join trips t on t.order_id = o.order_id 
              ------------------------------------------------------------
               union all 
                              select order_id  order_id,
                                     'destino' as ponto,
                                      dropoff_latitude  as latitude,
                                      dropoff_longitude as longitude
                             from trips t 
                            )a
limit 10")
```


<table>
<thead><tr><th scope=col>date</th><th scope=col>rate_code</th><th scope=col>?column?</th></tr></thead>
<tbody>
	<tr><td>2014-03-01</td><td>0         </td><td> 3.487179 </td></tr>
	<tr><td>2014-03-01</td><td>1         </td><td> 4.352112 </td></tr>
	<tr><td>2014-03-01</td><td>2         </td><td> 3.085553 </td></tr>
	<tr><td>2014-03-01</td><td>3         </td><td> 4.079655 </td></tr>
	<tr><td>2014-03-01</td><td>4         </td><td> 3.539362 </td></tr>
	<tr><td>2014-03-01</td><td>5         </td><td>11.047294 </td></tr>
	<tr><td>2014-03-02</td><td>0         </td><td> 2.912088 </td></tr>
	<tr><td>2014-03-02</td><td>1         </td><td> 4.022490 </td></tr>
	<tr><td>2014-03-02</td><td>2         </td><td> 3.068936 </td></tr>
	<tr><td>2014-03-02</td><td>3         </td><td> 3.974279 </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>date</th><th scope=col>faixa</th><th scope=col>corridas</th></tr></thead>
<tbody>
	<tr><td>2014-03-01</td><td>10        </td><td>114342    </td></tr>
	<tr><td>2014-03-01</td><td>+1hour    </td><td>   307    </td></tr>
	<tr><td>2014-03-01</td><td>20        </td><td> 68600    </td></tr>
	<tr><td>2014-03-01</td><td>30        </td><td> 20177    </td></tr>
	<tr><td>2014-03-01</td><td>40        </td><td>  5536    </td></tr>
	<tr><td>2014-03-01</td><td>50        </td><td>  1927    </td></tr>
	<tr><td>2014-03-01</td><td>60        </td><td>   659    </td></tr>
	<tr><td>2014-03-02</td><td>10        </td><td>103213    </td></tr>
	<tr><td>2014-03-02</td><td>+1hour    </td><td>    97    </td></tr>
	<tr><td>2014-03-02</td><td>20        </td><td> 53595    </td></tr>
</tbody>
</table>




<table>
<thead><tr><th scope=col>order_id</th><th scope=col>ponto</th><th scope=col>latitude</th><th scope=col>longitude</th><th scope=col>ordem</th></tr></thead>
<tbody>
	<tr><td>1140669  </td><td>origem   </td><td>40.72108 </td><td>-74.00855</td><td>1        </td></tr>
	<tr><td>1140689  </td><td>origem   </td><td>40.73393 </td><td>-73.98656</td><td>1        </td></tr>
	<tr><td>1140912  </td><td>origem   </td><td>40.76287 </td><td>-73.97936</td><td>1        </td></tr>
	<tr><td>1141009  </td><td>origem   </td><td>40.73199 </td><td>-73.99022</td><td>1        </td></tr>
	<tr><td>1141045  </td><td>origem   </td><td>40.75784 </td><td>-73.97845</td><td>1        </td></tr>
	<tr><td>1141509  </td><td>origem   </td><td>40.75950 </td><td>-73.98036</td><td>1        </td></tr>
	<tr><td>1142046  </td><td>origem   </td><td>40.72530 </td><td>-73.99632</td><td>1        </td></tr>
	<tr><td>1142453  </td><td>origem   </td><td>40.76229 </td><td>-73.97123</td><td>1        </td></tr>
	<tr><td>1142526  </td><td>origem   </td><td>40.78474 </td><td>-73.95614</td><td>1        </td></tr>
	<tr><td>1142717  </td><td>origem   </td><td>40.72118 </td><td>-73.99371</td><td>1        </td></tr>
</tbody>
</table>



## Visualização dos dados - Dashboard

Para visualizar o dashboard no Tableau Public [clique aqui](https://public.tableau.com/profile/hully6961#!/vizhome/challenge2v/ChallengeDashboard?publish=yes)

## Análise final e conclusão

Performance

Maio apresenta queda no número de corridas, uma diferença de quase 90% comparado ao mês anterior. Em  parte pode ser explicado pela variação da taxa de conversão nesse mês,outros motivos seriam a presença de nova concorrência,preço de viagens mais caras por conta do aumento dos custos, mas faltam dados para uma conclusão. 

No gráfico também é apresentado a frequência de corridas Percebe-se que maior parte da base realizou apenas entre 1 a 2 viagens. 

É preciso criar um plano estratégico para fidelizar clientes e aumentar a frequência com que o serviço é utilizado, isso pode proporcionar também o aumento de corridas e clientes ativos.


![Performance.PNG](attachment:Performance.PNG)

Receita

Como resultado na queda de corridas, a receita também é impactada (menor resultado do trimestre)
Quando observada por bandeira (produto), 90% é proveniente de *standart rate* que também gera menor ticket ,$11, por viagem tornando o produto muito menos rentável em comparação com JFK,$52 (segundo em representatividade da receita).

O ticket médio variou pouco, alcançando o pico de $10 em Maio, atenuando os efeitos negativos da queda de clientes ativos.

É preciso manter ticket médio e explorar retenção de clientes, principalmente daqueles que utilizam  serviços com maior ganho médio.

![receita.PNG](attachment:receita.PNG)

Operação

Para ter qualidade e atender de forma rápida, é necessário estar focado nos horários de maior movimento e traçar estratégias competitivas tanto em questão de preço/milha quanto na questão de alocação dos motoristas que estão pela cidade.

Alocação acertivas podem ser feitas com base nos horários de pico. 
Também é possível criar melhorias no aplicativo em relação aos motoristas (oferta e demanda) conforme o volume de chamadas por região.
Além disso, observando que o tempo médio de corridas está entre 10 e 20 minutos, novos produtos podem ser criados como mini viagens a preços competitivos.


![Opera%C3%A7%C3%A3o.PNG](attachment:Opera%C3%A7%C3%A3o.PNG)


```R

```
