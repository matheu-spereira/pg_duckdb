# pg_duckdb
PostgreSQL + DuckDB
pg_duckdb é uma extensão do Postgres que incorpora o mecanismo de análise colunar-vetorizada e os recursos do DuckDB no Postgres. Recomendamos usar o pg_duckdb para criar análises de alto desempenho e aplicativos com uso intensivo de dados.


# Estrutura
![image](https://github.com/user-attachments/assets/e4f02291-8c99-40c2-a2e0-555aca0b5272)

# BenchMark
![image](https://github.com/user-attachments/assets/0b7f59b3-7446-4090-abe2-ec5a977a716e)


# Configurando o banco para ler tabelas delta no minIO

#### Instala extensão delta
```
SELECT duckdb.install_extension('delta');
```

#### Ler extensões instaladas
```
select * from  duckdb.extensions 
```

#### Insere secret
```
INSERT INTO duckdb.secrets
(type, key_id, secret, session_token, region, endpoint, use_ssl, url_style)
VALUES ('S3', 'id', 'secret', '', 'us-east-1', 'minio:9000', false, 'path');
```

#### Verifica Secret
```
select * from  duckdb.secrets
```

#### Ler tabela delta do minio
```
SELECT count(*) FROM delta_scan('s3://bucket/pasta/pasta');
```

#### Cria View
```
CREATE VIEW duckdb.fat AS
	SELECT r['col1'] as col1, 
		     r['col2'] as col2, 
	FROM delta_scan('s3://bucket/pasta/pasta') r;
```

#### Ler view
```
select * from fat
```
