tructure SQL recommandée pour Smart Mobility Clermont
📌 Schéma : mobility
1. Table : traffic_data
sql
CREATE TABLE mobility.traffic_data (
    id SERIAL PRIMARY KEY,
    date DATE,
    heure TIME,
    axe VARCHAR(255),
    debit INT,
    vitesse_moyenne FLOAT,
    saturation FLOAT
);
2. Table : pollution_data
sql
CREATE TABLE mobility.pollution_data (
    id SERIAL PRIMARY KEY,
    date DATE,
    heure TIME,
    no2 FLOAT,
    pm10 FLOAT,
    pm25 FLOAT,
    indice_atmo INT
);
3. Table : meteo_data
sql
CREATE TABLE mobility.meteo_data (
    id SERIAL PRIMARY KEY,
    date DATE,
    heure TIME,
    temperature FLOAT,
    pluie FLOAT,
    vent FLOAT,
    humidite FLOAT
);
4. Table : accidents_data
sql
CREATE TABLE mobility.accidents_data (
    id SERIAL PRIMARY KEY,
    date DATE,
    heure TIME,
    gravite INT,
    latitude FLOAT,
    longitude FLOAT,
    meteo VARCHAR(50),
    type_route VARCHAR(100)
);
5. Table : transport_gtfs
sql
CREATE TABLE mobility.transport_gtfs (
    id SERIAL PRIMARY KEY,
    stop_id VARCHAR(50),
    stop_name VARCHAR(255),
    route_id VARCHAR(50),
    trip_id VARCHAR(50),
    arrival_time TIME,
    departure_time TIME,
    latitude FLOAT,
    longitude FLOAT
);
6. Table : zones_insee
sql
CREATE TABLE mobility.zones_insee (
    id SERIAL PRIMARY KEY,
    quartier VARCHAR(255),
    population INT,
    densite FLOAT,
    entreprises INT
);
🧩 Et ensuite : la table finale (Master Table)
sql
CREATE TABLE mobility.mobility_master AS
SELECT
    t.date,
    t.heure,
    t.axe,
    t.debit,
    t.vitesse_moyenne,
    p.no2,
    p.pm10,
    p.pm25,
    m.temperature,
    m.pluie,
    m.vent,
    a.gravite,
    z.quartier
FROM mobility.traffic_data t
LEFT JOIN mobility.pollution_data p USING (date, heure)
LEFT JOIN mobility.meteo_data m USING (date, heure)
LEFT JOIN mobility.accidents_data a USING (date, heure)
LEFT JOIN mobility.zones_insee z ON t.axe = z.quartier;