A continuació us mostrarem com hem importat les dades en la BD de Neo4j del projecte. Mitjançant un script en cypher
que carrega totes les dades, genera tots els nodes, relacions i afegeix les característiques on pertoca.

# NODES INDIVIDU
LOAD CSV WITH HEADERS FROM 'https://docs.google.com/spreadsheets/d/e/2PACX-1vTfU6oJBZhmhzzkV_0-avABPzHTdXy8851ySDbn2gq32WwaNmYxfiBtCGJGOZsMgCWjzlEGX4Zh1wqe/pub?output=csv' as row
WITH row
WHERE row.Id is not NULL
MERGE (i:Individu {IndividuID: row.Id})
SET i.Year = toInteger(row.Year), i.Name = row.name, i.Surname = row.surname, i.SecondSurname = row.second_surname

# NODES HABITATGE

# RELACIONS VIU

LOAD CSV WITH HEADERS FROM 'https://docs.google.com/spreadsheets/d/e/2PACX-1vRM4DPeqFmv7w6kLH5msNk6_Hdh1wuExRirgysZKO_Q70L21MKBkDISIyjvdm8shVixl5Tcw_5zCfdg/pub?output=csv' AS vivencia 
WITH vivencia
MATCH (p:Individu {IndividuID:vivencia.IND})
MATCH (h:Habitatge{LlarID:toInteger(vivencia.HOUSE_ID),Municipi:vivencia.Location,YearPadro:toInteger(vivencia.Year)})
merge (p) -[rel:VIU_A]- (h)
return count(rel)

# RELACIONS SAME_AS

# RELACIONS FAMILIA

LOAD CSV WITH HEADERS FROM 'https://docs.google.com/spreadsheets/d/e/2PACX-1vRVOoMAMoxHiGboTjCIHo2yT30CCWgVHgocGnVJxiCTgyurtmqCfAFahHajobVzwXFLwhqajz1fqA8d/pub?output=csv' AS row
WITH row.ID_1 AS Id_1, row.ID_2 AS Id_2, row.Relacio AS relacio, row.Relacio_Harmonitzada AS relacio_harmonitzada
MATCH (p:Individu {IndividuID:Id_1}) 
MATCH (o:Individu {IndividuID:Id_2})
MERGE (o)-[rel:Relacio_Familiar {relacio: relacio, relacio_harmonitzada: relacio_harmonitzada}]->(p)
RETURN count(rel);
