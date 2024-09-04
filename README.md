# db-gestore-eventi
1. Selezionare tutti gli eventi gratis, cioè con prezzo nullo (19)

   ``` SQL
   SELECT * FROM events WHERE price IS NULL;

1. Selezionare tutte le location in ordine alfabetico (82)

   ``` SQL
   SELECT * FROM locations ORDER BY name ASC;

1. Selezionare tutti gli eventi che costano meno di 20 euro e durano meno di 3 ore (38)

   ```SQL
   SELECT * FROM events WHERE price < 20 AND TIME_TO_SEC(duration) < 3 * 3600;

1. Selezionare tutti gli eventi di dicembre 2023 (25)

   ```SQL
   SELECT * FROM events WHERE MONTH(start) = 12 AND YEAR(start) = 2023;

1. Selezionare tutti gli eventi con una durata maggiore alle 2 ore (823)

   ``` SQL
   SELECT * FROM events WHERE HOUR(duration) > 2;

1. Selezionare tutti gli eventi, mostrando nome, data di inizio, ora di inizio, ora di fine e durata totale (1040)

   ```SQL
   SELECT name, DATE(start) as start_date, TIME(start) as start_time, DATE(end_of_sale) as end_date, TIME(end_of_sale) as end_time, duration FROM events;

1. Selezionare tutti gli eventi aggiunti da “Fabiano Lombardo” (id: 1202) (132)

   ``` SQL
   SELECT * FROM events WHERE user_id = 1202;

1. Selezionare il numero totale di eventi per ogni fascia di prezzo (81)

   ```SQL
   SELECT price, COUNT(*) AS total_events FROM events GROUP BY price;

1. Selezionare tutti gli utenti admin ed editor (9)

   ```SQL
   SELECT * FROM users WHERE role_id IN (1, 2);

1. Selezionare tutti i concerti (eventi con il tag “concerti”) (72)

   ```SQL
   SELECT * FROM `event_tag` WHERE tag_id = 1;

1. Selezionare tutti i tag e il prezzo medio degli eventi a loro collegati (11)

   ```SQL
   SELECT t.name AS tag_nome, AVG(e.price) AS prezzo_medio FROM tags t JOIN event_tag et ON t.id = et.tag_id JOIN events e ON et.event_id = e.id GROUP BY t.id, t.name;


1. Selezionare tutte le location e mostrare quanti eventi si sono tenute in ognuna di esse (82)

   ```sql
   SELECT locations.name, COUNT(events.id) AS totale_eventi FROM locations LEFT JOIN events ON locations.id = events.location_id GROUP BY locations.name;


1. Selezionare tutti i partecipanti per l’evento “Concerto Classico Serale” (slug: concerto-classico-serale, id: 34) (30)

   ```SQL
   SELECT u.* FROM users u JOIN bookings b ON u.id = b.user_id WHERE b.event_id = 34 AND u.role_id = 4;

1. Selezionare tutti i partecipanti all’evento “Festival Jazz Estivo” (slug: festival-jazz-estivo, id: 2) specificando nome e cognome (13)

   ```SQL
   SELECT u.first_name, last_name FROM users u JOIN bookings b ON u.id = b.user_id WHERE b.event_id = 2 AND u.role_id = 4;

1. Selezionare tutti gli eventi sold out (dove il totale delle prenotazioni è uguale ai biglietti totali per l’evento) (18)

   ```SQL
   SELECT e.id, e.name, e.start, e.total_tickets, COUNT(b.user_id) AS total_prenotations FROM events e LEFT JOIN bookings b ON e.id = b.event_id GROUP BY e.id, e.name, e.start, e.total_tickets HAVING COUNT(b.user_id) = e.total_tickets;

1. Selezionare tutte le location in ordine per chi ha ospitato più eventi (82)

   ```SQL
   SELECT locations.name, COUNT(events.id) AS total_events FROM locations LEFT JOIN events ON locations.id = events.location_id GROUP BY locations.name ORDER BY total_events DESC;

1. Selezionare tutti gli utenti che si sono prenotati a più di 70 eventi (74)

   ```SQL
   SELECT user_id, COUNT(*) AS total_prenotations FROM bookings GROUP BY user_id HAVING total_prenotations > 70;

1. Selezionare tutti gli eventi, mostrando il nome dell’evento, il nome della location, il numero di prenotazioni e il totale di biglietti ancora disponibili per l’evento (1040)

   ```SQL
   COUNT(bookings.id)) AS biglietti_disponibili FROM events LEFT JOIN locations ON events.location_id = locations.id LEFT JOIN bookings ON events.id = bookings.event_id GROUP BY events.id, locations.name;
