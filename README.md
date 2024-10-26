<B>DAX FORMATER</B>

Site para Formatar as funções DAX – indentar 

https://www.daxformatter.com/ 

1 - COUNT 
Função não conta número de linhas vazias 

2 - COUNTROWS 

Função conta linhas vazias 

3 - DISTINCTCOUNT 

Conta valores distintos – Conta valores em branco 

4 - DISTINCTCOUNTNOBLANK 

Não conta valores em branco 

5 - COUNTBLANK 
Retorna quantos valores em branco há na coluna 

6 - ALL 

Serve para retornar toda tabela em uma nova tabela ou coluna. 

Para usar siga modelagem – nova tabela 

 	NOME MEDIDA = ALL(NOME_TABELA) 

Ou 

	ALL (NOME_TABELA[COLUNA]) 

Função muito utilizada em combinação com a CALCULATE. 

7 - FILTER 

Retorna uma tabela que foi filtrada ou Auxilia no Filtro. 

Tabela – modelagem – inserir – tabela 

	FILTER(fVendas,fVendas[código_loja]=”CL001”]) 
 

Ou como Auxiliar 

	COUNTROWS(FILTER(Tabela, tabela[coluna]=”Criterio”)) 

8 - RELATED (funciona com criação de colunas) 

Precisa do relacionamento ativo para funcionar. Cria uma coluna/similar ao PROCV 

	RELATED(nome_tabela[procura]) 

Nome tabela[procura] – procura resultados da tabela que está relacionada para retornar o resultado. 
Função só funciona na tabela fato que *(muitos) no relacionamento de 1(um) com a tabela dimensão. 
 

9 - RELATEDTABLE 

Funciona somente com relacionamento ativo. 

A função na tabela dimensão funcionará dentro de outra função 

	COUNTROWS(RELATEDTABLE(fVendas)) 
 
 10 - LOOKUPVALUE 

Pode trazer informação de tabela não relacionada 
	
 	LOOCKUPVALUE (dloja[NomeLoja],dloja[CodigoLoja], fVendas[CodLoja]) 
 

Dloja[Nomeloja] – Nome que você quer buscar o resultado, coluna retorno. 
dLoja[Codigo_Loja] – qual coluna você está pesquisando. 
fVendas[CodLoja] – Valor procurado 
 

11 - SWITCH 

Para textos 

Condição para textos 

	= SWITCH(dFunciionario[Departamento], 

		“Loja 04”, “Venda Loja 04”, 

		“Loja 03”, “Venda Loja 03”, 

		“Loja 02”, “Venda Loja 02”, 

		“Loja 01”, “Venda Loja 01”) 
 

Condição para números 

	= SWITCH(TRUE(), 

		Dfuncionario[índice_satisfacao]<=2,”Atenção”, 

		Dfuncionario[índice_satisfacao]<4,”Ok”,”Satisfeito”)
		) 

 

12 - AND 

Função permite colocar duas condições em uma condicional ou critério de filtro exemplo. Aceita somente duas condições, diferente de utilizar o && 

		SWITCH(TRUE(), 

			AND(dfuncionario[índice]<=2,dfuncionario[performance]=”Abaixo do Esperado”), “Entrar em Contato)
			) 

Usando o && 

			dfuncionario[índice]<= && dfuncionario[performance]=”Abaixo do Esperado”&& dfuncionarios[TempoEmpresa]<=2, “Entrar em Contato)) 
 
13 - OR 

Função permite colocar duas condições em uma condicional ou critério de filtro exemplo. Aceita somente duas condições, diferente de utilizar o || 

		SWITCH(TRUE(), 

			OR(dfuncionario[índice]<=2,dfuncionario[performance]=”Abaixo do Esperado”), “Entrar em Contato)
			) 

Usando o && 

			dfuncionario[índice]<= || dfuncionario[performance]=”Abaixo do Esperado”|| dfuncionarios[TempoEmpresa]<=2, “Entrar em Contato)) 

 14 - HASONEVALUE 

Quando você quer retornar um nome 

		IF(HASONEVALUE(dclientes[Nome_Cliente]), 
			VALUES(dClientes[Nome_Cliente]),””
			) 

15 - SELECTEDVALUE 

Alternativa para utilizar ao invés da IF com HASONEVALUE 

		SELECTEDVALUE(dClientes[Nome_cliente],””) 

 16 - TÍTULO DINÂMICO PARA GRÁFICOS 

		TÍTULO_DINAMICO = “Faturamento de “& SELECTEDVALUE(fVendas[Codigo_centro_Distribuicao],”Vários). 

Se selecionar vários itens aparecerá no título “Faturamento de Vários” 

Título que aparecerá vários itens selecionados 

		VAR CENTROS = CONCATENATEX( 
			VALUES (fVendas[Codigo_Centro_Distribuicao), 

					fVendas[Codigo_centro_Distribuicao],”,”, 
					fVendas[Codigo_Centro_Distribuicao], 
					ASC 

					) 

Return “Faturamento de “ & centros 

17 - TREATAS 

Capaz de criar um relacionamento virtual – não precisa de relacionamento inativo como USERELATIONSHIP 

		 TREATAS_FATURAMENTO_BUDGET =  
		
		            CALCULATE(SUM(CROSSJOIN_BUDGET[Budget]), 
		
		                        TREATAS(VALUES(dimCalendario[NOMEMES_ANO]), 
		
		                            CROSSJOIN_BUDGET[NOMEMES_ANO] 
		
		                        ) 
		
		            ) 

18  - CALCULATE

A única função capaz de mudar um contexto de filtro existente 

		 ProdutosClasseA = CALCULATE( 
					[margem%],dProdutos[Classificacao]=”Class A”) 
 
		FaturamentoEmpresa2 = CALCULATE( 
						[faturamento],NomedoCliente=”Empresa2”) 
		 
 
		FaturamentoClasseAB = CALCULATE([faturamento], 
					dProdutos [Classificacao] = “Class A” || 
		                                          dProdutos [Classificacao] = “Class B”) 
 

CALCULATE diversos filtros 
 
Diversos filtros em um campo 

		FaturamentoEmpresasX =CALCULATE ([faturamento], 
		NomedoCliente IN {“Empresa 1”, “Empresa 4”, “Empresa 6”, “Empresa 8”}) 
 
Todo argumento passado para uma CALCULATE, vai ser uma tabela ou um modificador de contexto 

Representatividade % com CALCULATE 

Primeiro crie a medida do Faturamento 

		Faturamento = SUM(fVendas[Receita]) 

Calcule o Faturamento Total 
 
		Faturamento_total = CALCULATE (Faturamento], 
		                                                  ALLSELECETD(fVendas) 
		                                               ) 
 

Calcule a representatividade 
 
		Faturamento % = DIVIDE([Faturamento],[Faturamento_total]) 
 


FUNÇÕES QUE TRABALHAM COM A CALCULATE 

 

19 - KEEPFILTERS

Evita a sobreposição de contexto. 
A função não sobrepõe os filtros 
		FaturamentoAB = CALCULATE([faturamento], 

					KEEPFILTERS( 
					dProdutos[Classificacao] IN {“CLASS A”,”CLASS B”} 
						)
					)
Exemplo:
    ![image](https://github.com/user-attachments/assets/d4b30b85-7bd0-48fe-8a1e-6557377dc404)

20 - USERELATIONSHIP 

Função é utilizada para ativar relacionamentos inativos, e eles são ativados somente no momento que a função está sendo chamada. 

É necessário que tenha um relacionamento inativo – linha tracejada 

 
			QTDE_ENTREGA = CALCULATE( 
							COUNTROWS(fVendas), 
								USERLATIONSHIP(dCalendario[Date], 
								fVendas[DataEntrega]) 
							) 

 
21 - CROSSFILTER 

Crossfilter consegue modificar a estrutura de relacionamento. Podemos passar um argumento de direção de um relacionamento ou desativar um relacionamento ativo. 

Não é possível ativar relacionamento com esta função exemplo: 

 

			QTDE_ESTADOS =  
					CALCULATE( 
						DISTINCTCOUNT(dcidades[Estado]), 
			                CROSSFILTER(fVendas[Cidade_Destino],dCidades[Codigo Cidade], 
			                                      Both) 
			                         ) 

22 - ALL_SELECT 

Aceita filtros externos de caixas de Seleção/Datas/ Listas. 

Fora do visual tabela 

Exemplo: 

		Faturamento Total = CALCULATE([faturamento], 
		                                     ALL_SELECT (fVendas) 
		                                     ) 

23 - VARIÁVEIS – Arquivo Variáveis 

Variáveis servem para duas coisas: 

1 – Facilitar a leitura e o entendimento do código 

2 – Performance 

 Exemplo: 

		Variável margem% =  
		       VAR faturamento = sum(fvendas[Receita]) 
		       VAR custo = sumx(fvendas, fvendas[qtd]*fvendas[custo]) 
		       VAR margem = vfaturamento – vcusto 
		       VAR vresultado = divide(margem,faturamento) 
		       RETURN vresultado 



FUNÇÕES DE INTELIGÊNCIA DE TEMPO

Sempre desativar do PowerBI Inteligência de Dados temporais 

Dentro de Arquivo Opções – Global e Opções do arquivo atual 

24 - TABELA CALENDÁRIO 

Ir em Modelagem – Nova Tabela 
dimCalendario =  

            var mindate = min(fVendas[DATA_COMPRA]) 

            var calendario =  

                         FILTER(CALENDARAUTO(), 

                         year([Date])>=year(mindate) 

 

                         ) 

            return 

             ADDCOLUMNS( 

                calendario, 

                "MES", FORMAT([Date],"mmm"), 

                "MES_NUM",MONTH([Date]), 

                "NOMEMES_ANO",FORMAT([Date],"mmm-YY"), 

                "TRIMESTRE","TRI "& FORMAT([Date],"Q"), 

                "DIA_SEMANA",FORMAT([Date],"DDD"), 

                "ANO",YEAR([Date]), 

                "ANO_MÊS",EOMONTH([Date],0) 

		) 

 

Coluna ANO_MÊS ir em Formato na barra de formatação e digitar YYYY-m 

Toda vez que que criar uma tabela calendário: 

Selecione a tabela 

Clique em Marcar como tabela Calendario e clique em Date 

Exemplo:
![image](https://github.com/user-attachments/assets/c677b248-fbbb-4e95-a007-5d726ab44a71)

27 - DATESYTD 

Faz o acumulado do ano e começa com o saldo de janeiro 

 

FATURAMENTO_ACUMULADO = CALCULATE( 
				[faturamento],DATESYTD(dCalendario[Date]) 
					) 

DatesQTD e DatesMTD – Funcionam de maneira parecidas, mudando o intervalo para trimestre e mês respectivamente. 

28 - TOTALYTD 

	TotalYTD([faturamento],dCalendario[Date]) 
 
Por traz dos panos ela já possui a CALCULATE – está na função de maneira implicita 

 
29 - SAMEPERIODLASTYEAR 

SAMEPERIOD =  

		CALCULATE([faturamento], 
				SAMEPERIODLASTYEAR(dcalendario[Date]) 
 
30 - YoY% 
 

		Divide = ([faturamento]-[sameperiod],[sameperiod]) 
 

Retorna o percentual de diferença do ano anterior. 

 

31 - DATEADD 

Função pode pegar faturamento de acordo com o período que você especificar 

 

	CALCULATE([Faturamento], 
	
	     DATEADD(dCalendario[Date],-1,Month) 

No exemplo acima o -1, pega um período anterior o Month específica que é o mês. 

Se fosse 2 meses anteriores a função seria: 

		CALCULATE([Faturamento], 
		
		     DATEADD(dCalendario[Date],-2,Month) 

Para pegar faturamento do ano anterior: 

		CALCULATE([Faturamento], 
		
		     DATEADD(dCalendario[Date],-1,Year) 


Este exemplo tem o mesmo comportamento do SAMEPERIODLASTYEAR 

 

32 - PREVIOUSMONTH 

		CALCULATE ([Faturamento], 
		PREVIOUSMONTH(dCalendario[Date])) 	

A função não se adapta ao contexto de filtro Dia, como a DATEADD 

 
33 - PARELLELPERIOD 

		CALCULATE([Faturamento], 
		  PARALLELPERIOD( 
		    dCalendario[Date],-1,MONTH)
		  ) 	

 34 - DATESBETWEEN 

		CALCULATE([Faturamento], 
		
		  DATESBTWEEN(dCalendario[Date], 
		
		      Max(dCalendario[Date]-30, 
		
		      Max(dCalendario[Date]) 
		) 

O cálculo retorna o valor acumulado dos últimos 30 dias. 

35 - DATESINPERIOD 

		CALCULATE([Faturamento], 
		      DATESINPERIOD(dCalendario[Date], 
		
		      Max(dCalendario[Date]), 
		
		      -30, 
		
		      Day) 
		) 

Se alterar o -30 e o Day, muda o período do cálculo. 
No exemplo ele pega os últimos 30 dias e faz o acumulado. 

 
36 - CÁLCULO MÉDIA MÓVEL 

 
      MEDIA_MOVEL = 
                    AVERAGEX( 

                      DATESINPERIOD(dCalendario[Date], 

                      Max(dCalendario[Date]),-30,Day), 

                      [faturamento] 
                                  ) 
 

 

 

37 - DATEDIFF 

Tira a diferença entre duas datas 

			INTERVALO DE DIAS = DATEDIFF( 
						fVendas[DtCompra], 
			
						fVendas[DtEntrega], 
						Day) 
			 
38 - OPENINGBALANCEMONTH 
Sempre pega o saldo do último dia do mês anterior 

		OPENINGBALANCEMONTH([Faturamento],dCalendario[Date]) 

 
39 - STARTOFMONTH 
Sempre pega o saldo do último dia do mês anterior 

		CALCULATE([faturamento], 
		  STARTOFMONTH(dCalendario[Date])
		  ) 	

Sempre retorna o saldo do primeiro dia do mês corrente. 

40 - ENDOFMONTH 
Sempre retorna o saldo do último dia do mês corrente

		CALCULATE([faturamento], 
		  ENDOFMONTH(dCalendario[Date])
		  ) 

 
41 - LASTDATE 

Retorna o último dia do contexto de filtro aplicado a um visual. 

		CALCULATE([faturamento], 
		  LASTDATE(dCalendario[Date)
		  ) 

FUNÇÕES DE TABELA 

42 - ADDCOLUMNS 

Função calendário consegue pegar uma tabela e adicionar novas colunas 

Exemplo usando como Tabela – adicionando colunas na tabela original. 

Clique no botão lateral esquerdo em Tabela 

No menu Ferramentas de Tabela – Nova tabela 

 Digite: 

		 ADD_COLUMNS_PRODU1 =  
		
		            ADDCOLUMNS( 
		
		                dprodutos, 
		
		                "faturamento",[Faturamento]) 

 

A função criará a coluna Faturamento (com a medida Faturamento) na tabela dProdutos. Retor a tabela original com a coluna a mais de faturamento 

 

Exemplo usando como Tabela personalizada – escolher uma coluna e adicionar outras 

Clique no botão lateral esquerdo em Tabela 

No menu Ferramentas de Tabela – Nova tabela 

Digite: 

		ADD_COLUMNS_PRODU2 = 
		
		ADDCOLUMNS( 
		
		VALUES(dProdutos[CATEGORIA_PRODUTO]), 
		
		    "Faturammento",[Faturamento], 
		
		      "Margem_produto",[Margem%], 
		
		      "Qtde_vendida",[#QtdVendida] 
		
		    ) 

Exemplo usando como Medida e tabela Virtual 

			ADD_COLUMNS_MEDIDA1 =  
			
			        VAR TABELA_VIRTUAL =ADDCOLUMNS( 
			
			                                VALUES(dProdutos[CATEGORIA_PRODUTO]), 
			
			                                "Faturammento",[Faturamento], 
			
			                                "Margem_produto",[Margem%], 
			
			                                "Qtde_virtual",[#QtdVendida] 
			
			                                    ) 
			
			        VAR RESULT =  
			
			                    SUMX(TABELA_VIRTUAL, 
			
			                    [Qtde_virtual] 
			
			                    ) 
			
			        RETURN RESULT 

 

Exemplo usando como Medida, condicional IF e tabela Virtual 

 
ADD_COLUMNS_MEDIDA2 =  

		                VAR TABELA_VIRTUAL = ADDCOLUMNS( 
		
		                                                VALUES(dProdutos[CATEGORIA_PRODUTO]), 
		
		                                                "Faturamento",[Faturamento], 
		
		                                                "Margem_prod",[Margem%], 
		
		                                                "qtde_virtual",[#QtdVendida] 
		
		                                        ) 
		
		                VAR RESULT = SUMX(TABELA_VIRTUAL, 
		
		                                IF([Margem_prod]>0.38,[qtde_virtual],0) 
		
		                                 
		
		                    ) 
		
		 
		
		                RETURN RESULT 

 
43 - SUMMARIZE 

Esta função retorna o agrupamento de algumas linhas, conforme as informações são inseridas na base de dados.
Vale ressaltar que ela não soma, não faz média, ela somente agrupa as informações repetidas. 

Evite utilizar cálculos na SUMMARIZE o ideal é adicionar a função ADDCOLUMNS 

 

Como tabela calculada 

Modelagem – Inserir Nova tabela 

Digite: 

 
		
		SUMMARIZE1 =  
		
		            SUMMARIZE(fVendas, 
		
		            fVendas[CODIGO_CENTRO_DISTRIBUICAO]) 

 

		SUMMARIZE2 =  
		
		            SUMMARIZE(fVendas, 
		
		            fVendas[CODIGO_CENTRO_DISTRIBUICAO], 
		
		            fVendas[CODIGO_CAT]) 

 

Como medida 

Média da quantidade de produtos vendidos por dia, visão por cliente 

			SUMMARIZE =  
	
	                    var produto = SUMMARIZE( 
	
	                        fVendas, 
	
	                        dProdutos[CATEGORIA_PRODUTO], 
	
	                        dimCalendario[Date] 
	
	                    ) 
	
	                    var produtos_diarios = 
	
	                        ADDCOLUMNS(produto, 
	
	                                "qtde",CALCULATE(sum(fVendas[QUANTIDADE])) 
	
	                                 ) 
	
	                    var result = 
	
	                        AVERAGEX( 
	
	                            produtos_diarios,[qtde] 
	
	                            ) 
	
	                     
	
	                    return result 

44 - CROSSJOIN 

 Função que junta duas tabelas, combinando os resultados. 

Como tabela 

CROSSJOIN =  

	                CROSSJOIN( 
	
	                    VALUES(dProdutos[CATEGORIA_PRODUTO]), 
	
	                    VALUES(fVendas[CODIGO_CENTRO_DISTRIBUICAO]) 
	
	                ) 

CROSSJOIN_BUDGET =  

	                    ADDCOLUMNS(CROSSJOIN( 
	
	                        VALUES(dimCalendario[NOMEMES_ANO]), 
	
	                        VALUES(fVendas[CODIGO_CENTRO_DISTRIBUICAO]) 
	
	                    ), 
	
	                    "Budget",IF(ISBLANK([SamePeriodLastYear]),[#faturamento],[SamePeriodLastYear]*1.02) 
	
	                    ) 

Come Medida 

 

	CROSS_JOIN_FAT =  
	
	                VAR TABELA_VIRTUAL =  
	
	                        CROSSJOIN( 
	
	                                VALUES(dProdutos[Classificao_IF]), 
	
	                                VALUES(dCidades[ESTADO]) 
	
	                        ) 
	
	                VAR TABELA_FILTRADA =  
	
	                                FILTER(TABELA_VIRTUAL, 
	
	                                    OR(dProdutos[Classificao_IF]="Class A", 
	
	                                    dCidades[ESTADO]="Rio de Janeiro" 
	
	                                    ) 
	
	                                ) 
	
	                VAR RESULT =  
	
	                            CALCULATE([Faturamento],TABELA_FILTRADA) 
	
	 
	
	             RETURN RESULT 

 
45 - ROWS 
 

Criar uma tabela Manual 

Modelagem – Nova Tabela 

	ROW("Nome", "Susie","Idade","40","Cidade","Campinas") 

 

46 - UNION 

Unindo duas tabelas – mesmo efeito que a mesclagem acrescentar tabela do Power Query 

		Tabela Manual =  
		
		UNION( 
		
		            ROW("Nome", "Susie","Idade","40","Cidade","Campinas"), 
		
		            ROW("Nome", "Bento","Idade","25","Cidade","Itatinga") 
		
		            ) 

 
47 - CALCULATETABLE 

Todas as funções que são combinadas com a CALCULATE podem ser usadas com a CALCULATETABLE 

		Como tabela calculada 
		
		CALCULATETABLE =  
		
		                CALCULATETABLE(fVendas,fVendas[CODIGO_CENTRO_DISTRIBUICAO]="SULX301") 

 

48 - INTERSECT 

Contagem de produtos distintos em relação ao mês anterior. Avalia o mês corrente e verifica quantos dos produtos distintos do mês selecionado foram também comprados no mês anterior. 

		INTERSECT =  
		
		            VAR vcomparacao = INTERSECT(VALUES(fVendas[CODIGO_CAT]), 
		
		                CALCULATETABLE(VALUES(fVendas[CODIGO_CAT]), 
		
		                                DATEADD(dimCalendario[Date],-1,MONTH) 
		
		                ) 
		
		            ) 
		
		            var vresult = COUNTROWS(vcomparacao) 
		
		            return vresult 

49 - EXCEPT 

Contagem de produtos distintos em relação ao mês anterior. Avalia o mês corrente e verifica quantos dos produtos distintos foram comprados no mês corrente e não foram comprados no mês passado. 

		EXCEPT =  
		
		            VAR vcomparacao = EXCEPT(VALUES(fVendas[CODIGO_CAT]), 
		
		                CALCULATETABLE(VALUES(fVendas[CODIGO_CAT]), 
		
		                                DATEADD(dimCalendario[Date],-1,MONTH) 
		
		                ) 
		
		            ) 
		
		            var vresult = COUNTROWS(vcomparacao) 
		
		            return vresult 

 

Usando a Except para contar novos clientes em relação ao ano anterior 

		NOVOS_CLIENTES =  
		
		            var vano = max(dimCalendario[ANO]) 
		
		            var vclientes = VALUES(fVendas[CODIGO_CLIENTE]) 
		
		            var vtodos_clientes =  
		
		                            CALCULATETABLE( 
		
		                                        VALUES(fVendas[CODIGO_CLIENTE]), 
		
		                                        dimCalendario[ANO]<vano 
		
		                            ) 
		
		            var vresult = COUNTROWS(EXCEPT(vclientes,vtodos_clientes)) 
		
		            return vresult 

 50 - GENERATESERIES 

Função cria cenários, para utilizá-la é necessário clicar em MODELAGEM – NOVO PARAMETRO – INTERVALO NUMÉRICO

![image](https://github.com/user-attachments/assets/79be0330-da17-41a0-92d3-30997fadbd0d)

Depois de dar um Criar nesta Tela o PowerBI acrescenta uma Segmentação de Dados, Adiciona ao modelo de Dados uma tabela e uma medida 
![image](https://github.com/user-attachments/assets/91e3fc73-57d1-4a87-9e38-9ca4d9dad50b)

A tabela gerada saíra de acordo com os intervalos que foram digitados em NOVO PARAMETRO Custo = GENERATESERIES(-0.2, 0.2, 0.05) 

 Ideal montar uma nova tabela para comparar o cenário atual com uma simulação 

Necessário criar medida para Simular o Custo 

	#SimulaçãoCusto =  
	
	            var custo =SUMX(fVendas,fVendas[PRECO_CUSTO_UNITARIO]*(1+Custo[Valor Custo])*fVendas[QUANTIDADE]) 
	
	            return  custo 

 
Necessário criar medida para Simular a Margem 

 
	#SimulaçãoMargem = var custo = [#SimulaçãoCusto] 
	
	                   var receita = sum(fVendas[RECEITA]) 
	
	                   var lucro = receita - custo 
	
	                   var margem = divide(lucro,receita) 
	
	 
	                  RETURN margem 

 

51 - RANKX 

Função para ranquear 

Sintaxe da Função: RANKX(Tabela, Expressão, [Valor],[Order],[Ties]) 
Tabela = ALL(dClientes) - Necessário colocar ALL devido aos filtros no Visual 

Expressão - [Faturamento] 

[Valor] - Item não obrigatório 
[Order] - DESC – para classificar do maior para o menor, ou ASC para classificar do menor para o maior 
[Ties] - Dense ou Skip – Item não é obrigatório - quando há empate uma opção apresenta o número subsequente outra opção não. 

		RANKX_1 = RANKX(ALL(dClientes[Nome_Cliente]),[Faturamento],,DESC) 

 52 - TOPN 

Função como tabela calculada 

Sintaxe (N Value, Tabela, [OrderbyExpression],[Order]) 
N Value –Ex: 5 – Ranqueará os 5 primeiros (produtos, clientes, salários) 

Tabela – qual tabela será ranqueada 
OrderbyExpression – A ordenação será feita com base em que medida 
[Order] - Crescente ou decrescente 

 
		TOPN = TOPN(5,dProdutos,[#faturamento],DESC) 

Como medida 

Ver o faturamento considerando apenas os top5 produtos 

		TOPN =  
		
		        VAR vtop5 =TOPN(5,VALUES(fVendas[CODIGO_CAT]),[Faturamento],DESC) 
		
		        VAR vtopfaturamento = CALCULATE([Faturamento],vtop5) 
		
		        RETURN vtopfaturamento 

  

Consegue ver a representatividade dos TOP3 

		TOPN3_% =  
		
		        VAR vtop3 =TOPN(3,VALUES(fVendas[CODIGO_CAT]),[Faturamento],DESC) 
		
		        VAR vtopfaturamento = CALCULATE([Faturamento],vtop3) 
		
		        RETURN dIVIDE(vtopfaturamento,[Faturamento]) 

 

Ou 

		TOPN3_% =  
		
		        VAR vtop3 =TOPN(3,VALUES(fVendas[CODIGO_CAT]),[Faturamento],DESC) 
		
		        VAR vtopfaturamento = SUMX(vtop3,[Faturamento]) 
		
		        RETURN dIVIDE(vtopfaturamento,[Faturamento]) 

 

Combinando TOPN com a GENERATESERIES 

 

Modelagem – Novo Parâmetro - Intervalo Numérico 

![image](https://github.com/user-attachments/assets/e864e8a4-5a44-4141-92f5-58aa5e53fee4)

Foi criada uma tabela Calculada com a seguinte função 

 

	TopClientes = GENERATESERIES(0, 20, 1) 

 

Faça a seguinte alteração: 

 

	TopClientes = GENERATESERIES(0, COUNTROWS(dProdutos), 1) 
 

Volte então para a função TOPN que você criou e coloque o nome da medida do parâmetro que foi criada 

		VAR vtop3 =TOPN(3,VALUES(fVendas[CODIGO_CAT]),[Faturamento],DESC) 
		
		        VAR vtopfaturamento = SUMX(vtop3,[Faturamento]) 
		
		        RETURN dIVIDE(vtopfaturamento,[Faturamento]) 


