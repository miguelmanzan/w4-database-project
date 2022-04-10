# Building MySQL Database

## Introduction

In this project we are going to build a MySQL database for a video store from CSV files that have been provided to us, creating the tables and the relationships between these tables.

## Dependencies

![SQLalchemy](https://img.shields.io/badge/SQLalchemy-v1.4.22-green)
![python-dotenv](https://img.shields.io/badge/dotenv-v0.20.0-yellow)
![pandas](https://img.shields.io/badge/pandas-v1.3.4-blue)
![warnings](https://img.shields.io/badge/warnings-v1.0.0-red)
![numpy](https://img.shields.io/badge/numpy-v1.20.3-yellowgreen)

## Data sources

#### CSV files

* [actor.csv](./data/actor.csv)

* [category.csv](./data/category.csv)

* [film.csv](./data/film.csv)

* [inventory.csv](./data/inventory.csv)

* [language.csv](./data/language.csv)

* [old_HDD.csv](./data/old_HDD.csv)

* [rental.csv](./data/rental.csv)

## Entities

From the CSV information we can conclude that our database will have 7 entities:

* actor

* category

* film

* film_actor

* inventory

* language

* rental


## Relations between entities

#### film-languages

* Relation: One to many

```text
                          __________________
                         | film             |
                         |__________________|
                         | film_id PK       |
                         | title            |
                         | description      |
 ________________        | release_year     |
| language       |       | length           |
|________________| 1   * | rental_duration  |
| language_id PK |_______| language_id      |
| name           |       | rental_rate      |
|________________|       | replacement_cost |
                         | rating           |
                         | special_features |
                         | category_id      |
                         |__________________|

```

#### film-category

* Relation: One to many

```text
                          __________________
                         | film             |
                         |__________________|
                         | film_id PK       |
                         | title            |
                         | description      |
                         | release_year     |
                         | length           |
                         | rental_duration  |
                         | language_id      |
                         | rental_rate      |
 ________________        | replacement_cost |
|  category      |       | rating           |
|________________| 1   * | special_features |
| category_id    |_______| category_id      |
| name           |       |__________________|
|________________|
```

#### film-actor

* Relation: Many to many

```text
 _______________         ________________
| actor         |       | film_actor     |        __________________
|_______________| *   * |________________|       | film             |
| actor_id - PK |_______| actor_id PK    | *   * |__________________|
| first_name    |       | film_id        |_______| film_id PK       |
| last_name     |       |                |       | title            |
|_______________|       |________________|       | description      |
                                                 | release_year     |
                                                 | length           |
                                                 | rental_duration  |
                                                 | language_id      |
                                                 | rental_rate      |
                                                 | replacement_cost |
                                                 | rating           |
                                                 | special_features |
                                                 | category_id      |
                                                 |__________________|  

```

#### film-inventory

* Relation: One to many

```text
                            _________________
 __________________        | inventory       |
| film             |       |_________________|
|__________________|       | inventory_id PK |
| film_id PK       |_______| film_id         |
| title            |       | store_id        |
| description      |       |_________________|
| release_year     |
| length           |
| rental_duration  |
| language_id      |
| rental_rate      |
| replacement_cost |
| rating           |
| special_features |
| category_id      |
|__________________|  

```

#### inventory-rental

* Relation: One to many

```text
                           _______________
 _________________        | rental        |
| inventory       |       |_______________|
|_________________|       | rental_id PK  |
| inventory_id PK |_______| inventory_id  |
| film_id         |       | customer_id   |
| store_id        |       | return_date   |
|_________________|       | staff_id      |
                          |_______________|

```

## Final UML Diagram

This UML diagram that determines the entities and alll their relations.

```text
                         _________________
                        | rental          |       _________________
                        |_________________|      | inventory       |
                        | rental_id PK    | *  1 |_________________|
                        | inventory_id    |______| inventory_id PK |
                        | customer_id     |      | film_id         |____
                        | return_date     |      | store_id        |    | *
                        | staff_id        |      |                 |    |
                        |_________________|      |_________________|    |
 _______________         ________________                               |
| actor         |       | film_actor     |        __________________    |
|_______________| *   * |________________|       | film             |   |
| actor_id - PK |_______| actor_id PK    | *   * |__________________|   | 1
| first_name    |       | film_id        |_______| film_id PK       |___|
| last_name     |       |                |       | title            |
|_______________|       |________________|       | description      |
                         ________________        | release_year     |
                        | language       |       | length           |
                        |________________| 1   * | rental_duration  |
                        | language_id PK |_______| language_id      |
                        | name           |       | rental_rate      |
                        |________________|       | replacement_cost |
                         ________________        | rating           |
                        |  category      | 1   * | special_features |
                        |________________|   ____| category_id      |
                        | category_id    |__|    |__________________|  
                        | name           |         
                        |________________|

```

