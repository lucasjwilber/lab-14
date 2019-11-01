1. create a new table to start normalization:

CREATE TABLE BOOKSHELVES (id SERIAL PRIMARY KEY, name VARCHAR(255));

2. add a column for the new table's normalized data:

INSERT INTO bookshelves(name) SELECT DISTINCT bookshelf FROM books;

3. add a column in the old table to replace the column that is being normalized:

ALTER TABLE books ADD COLUMN bookshelf_id INT;

4. set the new table's data to be the normalized version of the old table's data:

UPDATE books SET bookshelf_id=shelf.id FROM (SELECT * FROM bookshelves) AS shelf WHERE books.bookshelf = shelf.name;

5. drop the now redundant column in the old table:

ALTER TABLE books DROP COLUMN bookshelf;

6. link the two new columns of normalized data between the two tables:

ALTER TABLE books ADD CONSTRAINT fk_bookshelves FOREIGN KEY (bookshelf_id) REFERENCES bookshelves(id);