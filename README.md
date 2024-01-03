# PosTech-Postgres

```
CREATE TABLE heroes (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    alterego VARCHAR(100),
    poderes TEXT,
    equipe VARCHAR(50),
    data_de_criacao DATE
);
```
```
INSERT INTO heroes (nome, alterego, poderes, equipe, data_de_criacao) VALUES ('Superman', 'Clark Kent', 'Super força, visão de raio-x, voo', 'Liga da Justiça', '1938-04-18'), ('Batman', 'Bruce Wayne', 'Inteligência, artes marciais, tecnologia avançada', 'Liga da Justiça', '1939-05-01'), ('Mulher-Maravilha', 'Diana Prince', 'Força sobre-humana, agilidade, braceletes mágicos', 'Liga da Justiça', '1941-12-01'), ('Flash', 'Barry Allen', 'Super velocidade, viagem no tempo', 'Liga da Justiça', '1940-01-01'), ('Aquaman', 'Arthur Curry', 'Comunicação com animais marinhos, super força', 'Liga da Justiça', '1941-11-01');
```
```
SELECT * FROM heroes ORDER BY nome;
```
```
SELECT equipe, COUNT(*) AS quantidade_de_herois
  FROM heroes
  GROUP BY equipe;
```
```
SELECT *
  FROM heroes
  WHERE equipe = 'Liga da Justiça' AND data_de_criacao < '1950-01-01';
```
```
CREATE TABLE poderes (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    descricao TEXT
  );

  CREATE TABLE herois_poderes (
      id_heroi INTEGER REFERENCES heroes(id),
      id_poder INTEGER REFERENCES poderes(id),
      PRIMARY KEY (id_heroi, id_poder)
  );
```
```
INSERT INTO poderes (nome, descricao)
VALUES
    ('Super força', 'Habilidade de levantar objetos pesados e destruir obstáculos com facilidade.'),
    ('Visão de raio-x', 'Capacidade de ver através de objetos sólidos.'),
    ('Voo', 'Habilidade de voar pelos céus com velocidade e destreza.'),
    ('Inteligência', 'Elevado nível de raciocínio lógico e capacidade estratégica.'),
    ('Artes marciais', 'Mestre em várias técnicas de luta corpo a corpo.'),
    ('Tecnologia avançada', 'Utilização de equipamentos e gadgets de alta tecnologia.');
```
```
INSERT INTO herois_poderes (id_heroi, id_poder)
VALUES
    (5, 1), (5, 3), -- Aquaman
    (2, 5), -- Batman
    (4, 2), (4, 6), -- Flash
    (3, 1), (3, 4), (3, 5), -- Mulher-Maravilha
    (1, 1), (1, 2), (1, 3); -- Superman
```
```
SELECT h.nome AS nome_heroi, p.nome AS poder_heroi
FROM heroes h
INNER JOIN herois_poderes hp ON h.id = hp.id_heroi
INNER JOIN poderes p ON hp.id_poder = p.id;
```
```
SELECT h.nome AS nome_heroi, p.nome AS poder_heroi
FROM heroes h
LEFT JOIN herois_poderes hp ON h.id = hp.id_heroi
LEFT JOIN poderes p ON hp.id_poder = p.id;
```
```
SELECT h.nome AS nome_heroi, p.nome AS poder_heroi
FROM heroes h
RIGHT JOIN herois_poderes hp ON h.id = hp.id_heroi
RIGHT JOIN poderes p ON hp.id_poder = p.id;
```
```
SELECT h.nome AS nome_heroi, p.nome AS poder_heroi
FROM heroes h
FULL JOIN herois_poderes hp ON h.id = hp.id_heroi
FULL JOIN poderes p ON hp.id_poder = p.id;
```
- Para conectar o pstgres com o spring
    - adicionar no properties
```
# datasource PostgreSQL
spring.datasource.url = jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=102030

# jpa
spring.jpa.database-plataform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.properties.hibernate.show_sql=true
spring.jpa.properties.hibernate.format_sql=true
```
spring.jpa.show-sql=true
