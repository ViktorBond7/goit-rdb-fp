-- 1-----------------------
create schema pandemic;
![](./img/P1_schema.png)

use pandemic;

-- 2-----------------------

CREATE TABLE countries (
country_code varchar(10) primary key not null,
country_name text not null
);

insert into countries (country_code, country_name)
select distinct Code, Entity
from infectious_cases;

select \* from infectious_cases;
![](./img/P2_cound_rows.png)

alter table infectious_cases
modify Code varchar(10);

alter table infectious_cases
ADD CONSTRAINT fk_country
FOREIGN KEY (country_code) REFERENCES countries(country_code);

![](./img/P2_EER_Diagram.png)

ALTER TABLE infectious_cases
CHANGE Code country_code VARCHAR(10);

alter table infectious_cases
drop Entity;

alter table infectious_cases
add column id int primary key auto_increment;

-- 3-----------------------

SELECT
c.country_code,
c.country_name,
avg(ic.Number_rabies) as avg_number_rabies,
min(ic.Number_rabies) as min_number_rabies,
max(ic.Number_rabies) as max_number_rabies,
sum(ic.Number_rabies) as sum_number_rabies
FROM countries c
JOIN infectious_cases ic
ON c.country_code = ic.country_code
where ic.Number_rabies is not null
and trim(coalesce(ic.Number_rabies, '')) <>''
group by c.country_code, c.country_code
order by avg_number_rabies DESC
limit 10;

![](./img/P3_1.png)

-- 4-----------------------

SELECT
Year,
MAKEDATE(Year, 1) AS full_date,
TIMESTAMPDIFF(Year, MAKEDATE(Year, 1), CURRENT_DATE()) as years_difference
FROM infectious_cases;

![](./img/P4_1.png)

-- 5-----------------------
