---
layout: post
title: "Rails Migration Types"
date: 2011-09-26 22:58
comments: true
categories: [rails, db, migration, ruby]
---

I have a hard time remembering what types can be used in various dbs.
Earlier today, I stumbled across a useful table of types for databases
that I don’t typically use and I thought I would share it with everyone.


 Rails      | MySQL   | Postgres  | sqlite   | mongoid
-------     |-------  |---------- |----------|---------
:binary     | blob    | bytea     | blob     | *
:boolean    | tinyint | boolean   | boolean  | Boolean
:date       | date    | date      | date     | Date
:datetime   | datetime| timestamp | datetime | DateTime
:decimal    | decimal | decimal   | decimal  | Float
:float      | float   | float     | float    | Float
:integer    | int(11) | intger    | intger   | Integer
:string     | varchar | *         | varchar  | String
:text       | text    | text      | text     | String
:time       | time    | time      | dattime  | Time
:timestamp  |datetime | timestamp | datetime | Time
