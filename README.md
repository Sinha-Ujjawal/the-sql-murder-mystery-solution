# The Sql Murder Mystery Solution
[The SQL Murder Mystery](https://mystery.knightlab.com/) Solution


```sql
-- Searching crime report
SELECT
    *
FROM crime_scene_report
WHERE
    city = 'SQL City'
AND type = 'murder'
AND date = '20180115' ;
-- description: Security footage shows that there were 2 witnesses.
-- The first witness lives at the last house on "Northwestern Dr".
-- The second witness, named Annabel, lives somewhere on "Franklin Ave".

-- Identifying the witnesses
SELECT
  *
FROM person
WHERE LOWER(address_street_name) LIKE '%northwestern%'
  ORDER BY address_number DESC
  LIMIT 1 ;
-- first witness
-- full name:  Morty Schapiro
-- street:     Northwestern Dr
-- ssn:        111564949
-- license_id: 118009
-- person_id:  14887

SELECT
  *
FROM person
WHERE LOWER(name) LIKE '%annabel%'
LIMIT 10 ;
-- second witness
-- full name:  Annabel Miller
-- street:     Franklin Ave.
-- ssn:        318771143
-- license_id: 490173
-- person_id:  16371


-- Interview information
-- first witness
SELECT
  *
FROM interview
WHERE person_id = 14887 ;
-- Transcript
-- I heard a gunshot and then saw a man run out.
-- He had a "Get Fit Now Gym" bag.
-- The membership number on the bag started with "48Z".
-- Only gold members have those bags.
-- The man got into a car with a plate that included "H42W".

-- second witness
SELECT
  *
FROM interview
WHERE person_id = 16371 ;
-- Transcript
-- I saw the murder happen, and I recognized the killer from my gym\
-- when I was working out last week on January the 9th.

SELECT
 gfnm.*
FROM get_fit_now_member gfnm
INNER JOIN person
ON gfnm.person_id = person.id
INNER JOIN drivers_license dl
ON person.license_id = dl.id
WHERE
  gfnm.id LIKE '48Z%'
  AND membership_status = 'gold'
LIMIT 10;
-- According to witness 1
-- Murderer is Jeremy Bowers
-- person_id: 67318

-- SELECT
--   *
-- FROM get_fit_now_check_in
-- WHERE check_in_date = '20180109'
-- LIMIT 10;

-- Interview suspect: Jeremy Bowers
SELECT
  *
FROM interview
WHERE person_id = 67318 ;
-- Transcript from the actual murderer
-- I was hired by a woman with a lot of money.
-- I don't know her name but I know she's around 5'5" (65") or 5'7" (67").
-- She has red hair and she drives a Tesla Model S.
-- I know that she attended the SQL Symphony Concert 3 times in December 2017.
-- But he is hired, time to find the actual Killer


SELECT
  person.id,
  person.name,
  person.license_id,
  person.address_street_name,
  person.ssn,
  dl.*
FROM person
INNER JOIN drivers_license dl
ON person.license_id = dl.id
WHERE
  hair_color = 'red'
  AND height BETWEEN 65 AND 67
  AND car_make = 'Tesla'
  AND car_model = 'Model S'
LIMIT 10 ;
-- Killer suspects
-- First suspect
-- full_name: Red Korb
-- person_id: 78881
-- ssn:       961388910
-- Second suspect
-- full_name: Regina George
-- person_id: 90700
-- ssn:       337169072
-- Third suspect
-- full_name: Miranda Priestly
-- person_id: 99716
-- ssn:       987756388


SELECT
  *
FROM facebook_event_checkin
WHERE
  person_id IN (78881, 90700, 99716)
LIMIT 10 ;
-- Miranda Priestly is the Killer!

```
