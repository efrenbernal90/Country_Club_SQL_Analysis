# Country_Club_SQL_Analysis
An exploratory SQL analysis of Country Club members and facility usage. This project follows along with the SQL Practice set found on https://pgexercises.com/ 

## Resources
- [Data](Resources/clubdata.sql)

## Additional Resources
- [PosgreSQL v.11 Documentation](https://www.postgresql.org/docs/11/index.html)
- [pgAdmin 4 v. 6.10](https://www.pgadmin.org/docs/pgadmin4/6.10/index.html)
- [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)

## Overview

The dataset for these exercises is for a newly created country club, with a set of members, 
facilities such as tennis courts, and booking history for those facilities. 
Amongst other things, the club wants to understand how they can use their information to analyse facility usage/demand. 
Please note: this dataset is designed purely for supporting an interesting array of exercises, and the database schema is flawed in several aspects - please don't take it as an example of good design. 
We'll start off with a look at the Members table:

```SQL
   CREATE TABLE cd.members
    (
       memid integer NOT NULL, 
       surname character varying(200) NOT NULL, 
       firstname character varying(200) NOT NULL, 
       address character varying(300) NOT NULL, 
       zipcode integer NOT NULL, 
       telephone character varying(20) NOT NULL, 
       recommendedby integer,
       joindate timestamp NOT NULL,
       CONSTRAINT members_pk PRIMARY KEY (memid),
       CONSTRAINT fk_members_recommendedby FOREIGN KEY (recommendedby)
            REFERENCES cd.members(memid) ON DELETE SET NULL
    );
```
Each member has an ID (not guaranteed to be sequential), basic address information, a reference to the member that recommended them (if any), and a timestamp for when they joined. The addresses in the dataset are entirely (and unrealistically) fabricated.

```SQL
    CREATE TABLE cd.facilities
    (
       facid integer NOT NULL, 
       name character varying(100) NOT NULL, 
       membercost numeric NOT NULL, 
       guestcost numeric NOT NULL, 
       initialoutlay numeric NOT NULL, 
       monthlymaintenance numeric NOT NULL, 
       CONSTRAINT facilities_pk PRIMARY KEY (facid)
    );
```
The facilities table lists all the bookable facilities that the country club possesses. The club stores id/name information, the cost to book both members and guests, the initial cost to build the facility, and estimated monthly upkeep costs. They hope to use this information to track how financially worthwhile each facility is.

```SQL
    CREATE TABLE cd.bookings
    (
       bookid integer NOT NULL, 
       facid integer NOT NULL, 
       memid integer NOT NULL, 
       starttime timestamp NOT NULL,
       slots integer NOT NULL,
       CONSTRAINT bookings_pk PRIMARY KEY (bookid),
       CONSTRAINT fk_bookings_facid FOREIGN KEY (facid) REFERENCES cd.facilities(facid),
       CONSTRAINT fk_bookings_memid FOREIGN KEY (memid) REFERENCES cd.members(memid)
    );
```
Finally, there's a table tracking bookings of facilities. This stores the facility id, the member who made the booking, the start of the booking, and how many half hour 'slots' the booking was made for.
