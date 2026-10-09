# BSUIR-Data-Models-and-Database-Managment-Systems-DM-DMS-


Багаев Леонид Сергеевич
Группа 453501
Тема: База данных для многопользовательской онлайн игры Conquerors Blade


# Сущности базы данных

## users

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| id | integer | PK, increment | Уникальный идентификатор пользователя |
| username | string | not null, unique | Имя пользователя |
| password | integer | not null | Пароль |
| permission | user_permission_level | — | Уровень прав доступа |

## characters

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| id | int | PK, increment | Уникальный идентификатор персонажа |
| user_id | integer | FK → users.id | Владелец персонажа |
| name | string | not null | Имя персонажа |
| weapon_id | integer | FK → weapons.id | Экипированное оружие |

## weapons

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| id | int | PK, increment | Уникальный идентификатор оружия |
| type | string | not null | Тип оружия |
| damage | float | — | Урон |
| penetration | float | — | Пробитие брони |

## unit_holders

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| id | int | PK, increment | Уникальный идентификатор записи |
| character_id | integer | FK → characters.id | Персонаж-владелец |
| unit_id | integer | FK → units.id | Юнит |
| doctrine_1_id | integer | FK → doctrines.id | Трактат 1 |
| doctrine_2_id | integer | FK → doctrines.id | Трактат 2 |
| doctrine_3_id | integer | FK → doctrines.id | Трактат 3 |
| doctrine_4_id | integer | FK → doctrines.id | Трактат 4 |
| doctrine_5_id | integer | FK → doctrines.id | Трактат 5 |

## units

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| id | int | PK, increment | Уникальный идентификатор юнита |
| name | string | — | Название юнита |
| type | unit_type | — | Тип юнита |

## doctrines

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| id | int | PK, increment | Уникальный идентификатор трактата |
| name | string | — | Название трактата |
| rank | integer | check: rank >= 1 and rank <= 5 | Ранг трактата (1–5) |

## matches

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| match_id | int | PK, increment | Уникальный идентификатор матча |
| length | timestamp | — | Длительность матча |
| map_id | integer | FK → maps.id | Карта матча |
| winner | side | — | Победившая сторона |

## match_partisipants

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| id | int | PK, increment | Уникальный идентификатор записи |
| character_id | integer | FK → characters.id | Персонаж-участник |
| match | integer | FK → matches.match_id | Матч |
| kills | integer | — | Убийства |
| deaths | integer | — | Смерти |
| assists | integer | — | Помощь |
| score | integer | — | Очки |
| win | boolean | — | Победа |

## guilds

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| id | int | PK, increment | Уникальный идентификатор гильдии |
| name | varchar(50) | not null | Название гильдии |

## guild_memberships

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| id | int | PK, increment | Уникальный идентификатор записи |
| character_id | integer | FK → characters.id | Персонаж-участник |
| guild_id | integer | FK → guilds.id | Гильдия |
| position | guild_position | — | Должность в гильдии |
| — | — | (guild_id, character_id) unique | Уникальная пара: персонаж в гильдии один раз |
| — | — | Note: 'maximum 50 guild members' | Максимум 50 участников в гильдии |

## maps

| Поле | Тип | Ограничения | Описание |
|------|-----|-------------|----------|
| id | int | PK, increment | Уникальный идентификатор карты |
| name | string | — | Название карты |
| play_mode | map_play_mode | — | Режим игры |
| is_active | boolean | default: 0 | Активна ли карта |

---

## Enum-типы

### user_permission_level

| Значение | Описание |
|----------|----------|
| admin | Администратор |
| moderator | Модератор |
| player | Игрок |

### side

| Значение | Описание |
|----------|----------|
| attack | Атакующая сторона |
| defence | Защищающаяся сторона |

### unit_type

| Значение | Описание |
|----------|----------|
| infantry | Пехота |
| archer | Лучники |
| cavalry | Кавалерия |

### guild_position

| Значение | Описание |
|----------|----------|
| member | Участник |
| officer | Офицер |
| guildmaster | Глава гильдии |

### map_play_mode

| Значение | Описание |
|----------|----------|
| siege | Осада |
| conquer | Завоевание |
| PvE | Игра против ИИ |