# Building MySQL Database

## Introduction

In this project we are going to build a MySQL database for a video store from CSV files that have been provided to us, creating the tables and the relationships between these tables.

## Dependencies

![SQLalchemy](https://img.shields.io/badge/SQLalchemy-v1.4.22-green)
![python-dotenv](https://img.shields.io/badge/dotenv-v0.20.0-yellow)
![pandas](https://img.shields.io/badge/pandas-v1.3.4-blue)
![warnings](https://img.shields.io/badge/warnings-v1.0.0-red)
![numpy](https://img.shields.io/badge/numpy-v1.20.3-yellowgreen)
![faker](https://img.shields.io/badge/numpy-v8.8.1-blueviolet)

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
|__________________| 1   * | inventory_id PK |
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
|_________________| 1   * | rental_id PK  |
| inventory_id PK |_______| inventory_id  |
| film_id         |       | customer_id   |
| store_id        |       | return_date   |
|_________________|       | staff_id      |
                          |_______________|

```

#### BONUS 1: customer-rental

* Relation: One to many

```text
                           _______________
                          | rental        |
 _________________        |_______________|
| customer        |       | rental_id PK  |
|_________________| 1   * | inventory_id  |
| customer_id  PK |_______| customer_id   |
| first_name      |       | return_date   |
| last_name       |       | staff_id      |
| address         |       |_______________|
| phone           |
|_________________|

```

#### BONUS 2: staff-rental

* Relation: One to many

```text
                           _______________
                          | rental        |
                          |_______________|
                          | rental_id PK  |
 _________________        | inventory_id  |
| staff           |       | customer_id   |
|_________________| 1   * | return_date   |
| staff_id  PK    |_______| staff_id      |
| first_name      |       |_______________|
| last_name       |
| address         |
| phone           |
|_________________|

```

#### BONUS 3: inventory-store

* Relation: One to many

```text
 _________________           ______________
| inventory       |         | store        |
|_________________|       1 |______________|
| inventory_id PK |   ______| store_id PK  |
| film_id         |  |      | address      |
| store_id        |__|      | phone        |
|                 | *       | cif          |
|_________________|         |______________|

```

## Final UML Diagram

This UML diagram that determines the entities and alll their relations.

```text
                         _________________
                        | rental          |       _________________           ______________
 _______________        |_________________|      | inventory       |         | store        |
| customer      |       | rental_id PK    | *  1 |_________________|       1 |______________|
|_______________| 1   * | inventory_id    |______| inventory_id PK |   ______| store_id PK  |
| customer_id PK|_______| customer_id     |      | film_id         |__|_     | address      |
| first_name    |       | return_date     |      | store_id        |__| | *  | phone        |
| last_name     |  _____| staff_id        |      |                 | *  |    | cif          |
| address       | |   * |_________________|      |_________________|    |    |______________|
| phone         | |      ________________                               |
|_______________| |     | film_actor     |        __________________    |
                  |   * |________________|       | film             |   |
 _______________  |  ___| actor_id PK    | *   * |__________________|   | 1
| staff         | | |   | film_id        |_______| film_id PK       |___|
|_______________| | |   |                |       | title            |
| staff_id  PK  |_| |   |________________|       | description      |
| first_name    | 1 |    ________________        | release_year     |
| last_name     |   |   | language       |       | length           |
| address       |   |   |________________| 1   * | rental_duration  |
| phone         |   |   | language_id PK |_______| language_id      |
|_______________|   |   |  name          |       | rental_rate      |
 _______________    |   |________________|       | replacement_cost |
| actor         |   |    ________________        | rating           |
|_______________|   |   | category       | 1   * | special_features |
| actor_id      |___|   |________________|   ____| category_id      |
| first_name    | *     | category_id    |__|    |__________________|  
| last_name     |       | name           |         
|_______________|       |________________|

```

