A continuació us mostrarem com hem importat les dades en la BD de Neo4j del projecte. Mitjançant un script en cypher
que carrega totes les dades, genera tots els nodes, relacions i afegeix les característiques on pertoca.

# NODES INDIVIDU
LOAD CSV WITH HEADERS FROM 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTfU6oJBZhmhzzkV_0-avABPzHTdXy8851ySDbn2gq32WwaNmYxfiBtCGJGOZsMgCWjzlEGX4Zh1wqe/pub?output=csv' as row
WITH row
WHERE row.Id is not NULL
MERGE (i:Individu {IndividuID: row.Id})
SET i.Year = toInteger(row.Year), i.Name = row.name, i.Surname = row.surname, i.SecondSurname = row.second_surname

# NODES HABITATGE
LOAD CSV WITH HEADERS FROM "https://docs.google.com/spreadsheets/d/e/2PACX-1vT0ZhR6BSO_M72JEmxXKs6GLuOwxm_Oy-0UruLJeX8_R04KAcICuvrwn2OENQhtuvddU5RSJSclHRJf/pub?output=csv" AS row
WITH row.Municipi AS Municipi, toInteger(row.Id_Llar) AS LlarID, toInteger(row.Any_Padro) AS YearPadro, row.Carrer AS Carrer, toInteger(row.Numero) AS Num 
FOREACH ( ignoreMe in CASE WHEN LlarID is not null THEN [1] ELSE [] END | 
FOREACH (ignoreMe in CASE WHEN Municipi is not null THEN [1] ELSE [] END |
MERGE (h:Habitatge {LlarID:LlarID}) SET h.Municipi = Municipi, h.YearPadro = YearPadro, h.Carrer = Carrer, h.Num = Num))

CREATE INDEX HabitatgeID FOR (h:Habitatge) ON (h.LlarID, h.Municipi, h.YearPadro)

CREATE CONSTRAINT UniqueHabitatgeID FOR (h:Habitatge) REQUIRE h.HabitatgeID IS UNIQUE

# RELACIONS VIU

LOAD CSV WITH HEADERS FROM 'https://docs.google.com/spreadsheets/d/e/2PACX-1vRM4DPeqFmv7w6kLH5msNk6_Hdh1wuExRirgysZKO_Q70L21MKBkDISIyjvdm8shVixl5Tcw_5zCfdg/pub?output=csv' AS vivencia 
WITH vivencia
MATCH (p:Individu {IndividuID:vivencia.IND})
MATCH (h:Habitatge{LlarID:toInteger(vivencia.HOUSE_ID),Municipi:vivencia.Location,YearPadro:toInteger(vivencia.Year)})
merge (p) -[rel:VIU_A]- (h)
return count(rel)

# RELACIONS SAME_AS

LOAD CSV WITH HEADERS FROM 
"https://docs.google.com/spreadsheets/d/e/2PACX-1vTgC8TBmdXhjUOPKJxyiZSpetPYjaRC34gmxHj6H2AWvXTGbg7MLKVdJnwuh5bIeer7WLUi0OigI6wc/pub?output=csv" AS row 
WITH row.Id_A AS Id_A, row.Id_B AS Id_B
MATCH (p:Individu {IndividuID:Id_A}) 
MATCH (o:Individu {IndividuID:Id_B})
MERGE (o)-[rel: SAME_AS]-(p)
RETURN count(rel)

# RELACIONS FAMILIA

LOAD CSV WITH HEADERS FROM 'https://docs.google.com/spreadsheets/d/e/2PACX-1vRVOoMAMoxHiGboTjCIHo2yT30CCWgVHgocGnVJxiCTgyurtmqCfAFahHajobVzwXFLwhqajz1fqA8d/pub?output=csv' AS row
WITH row.ID_1 AS Id_1, row.ID_2 AS Id_2, row.Relacio AS relacio, row.Relacio_Harmonitzada AS relacio_harmonitzada
MATCH (p:Individu {IndividuID:Id_1}) 
MATCH (o:Individu {IndividuID:Id_2})
MERGE (o)-[rel:Relacio_Familiar {relacio: relacio, relacio_harmonitzada: relacio_harmonitzada}]->(p)
RETURN count(rel);
