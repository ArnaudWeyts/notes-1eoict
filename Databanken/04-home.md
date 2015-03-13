Databases labo 4
=================

Haroen Viaene

1. Geef de naam en de prijs van de meest verkochte optie. Geef tevens ook het aantal keren dat deze optie verkocht werd (in dezelfde query). 

    ```SQL
    SELECT naam, prijs, MAX(aantal_verkocht)
    FROM(
        SELECT naam, prijs, COUNT(autos_chassisNR) AS aantal_verkocht
        FROM opties
        INNER JOIN autos_has_opties ON autos_has_opties.opties_id = opties.id
        GROUP BY opties_id
    ) AS t;
    ```
    OF
    
    ```SQL
    SELECT naam, prijs, count(autos_chassisNR) AS aantal
    FROM opties
    INNER JOIN autos_has_opties ON opties_id=id
    GROUP by opties_id
    ORDER BY aantal DESC limit 1;
    ```
2. Geef het aantal verschillende modellen van auto’s die verkocht werden door verkoper Bram. 

    ```SQL
    SELECT COUNT(DISTINCT model) AS aantal_versch_modellen
    FROM autoinfo
    INNER JOIN autos ON autos.autoinfo_id = autoinfo.id
    INNER JOIN verkopers ON verkopers.id = verkopers_id
    WHERE verkopers.naam = 'Bram';
    ```

3. Geef de naam van de verkoper die nog geen enkele wagen verkocht heeft. Doe dit aan de hand van een subquery.

    ```SQL
    SELECT naam
    FROM verkopers
    WHERE id NOT IN
    (
        SELECT verkopers_id
        FROM autos
    );
    ```

4. Geef de naam van de verkoper die nog geen enkele wagen verkocht heeft. Doe dit zonder gebruik te maken van een subquery.

    ```SQL
    SELECT naam
    FROM verkopers
    LEFT JOIN autos ON autos.verkopers_id = verkopers.id
    GROUP BY verkopers_id
    HAVING COUNT(verkopers_id) = 0;
    ```

5. Geef de totale verkoopprijs (basisprijs + prijs van de opties) voor elke wagen die voorzien is van 2 of meer opties.

    ```SQL
    SELECT merk, model,basisprijs+SUM(prijs) AS totale_prijs
    FROM autoinfo
    INNER JOIN autos ON autoinfo_id = autoinfo.id
    INNER JOIN autos_has_opties ON autos_chassisNR = chassisNR
    INNER JOIN opties ON opties.id = opties_id
    GROUP by autos_chassisNR
    HAVING COUNT(*) > 1;
    ```
