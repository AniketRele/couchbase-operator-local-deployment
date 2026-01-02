6️⃣ Create Scope & Collection (Modern Data Model)

In Query Workbench (N1QL):

CREATE SCOPE `default`.app;
CREATE COLLECTION `default`.app.users;


Insert sample data:

INSERT INTO `default`.app.users (KEY, VALUE)
VALUES
("user::1", {"name": "Aniket", "role": "Data Engineer"}),
("user::2", {"name": "Alex", "role": "Analyst"});


Create index:

CREATE PRIMARY INDEX ON `default`.app.users;

7️⃣ Analytics: Dataverse & Dataset

Switch to Analytics → Workbench.

CREATE DATAVERSE demo_analytics;
USE demo_analytics;

CREATE DATASET users
ON `default`.app.users;

CONNECT LINK Local;


Verify ingestion:

SELECT COUNT(*) FROM users;
SELECT * FROM users LIMIT 5;

8️⃣ Run Analytics Queries
SELECT role, COUNT(*) AS cnt
FROM users
GROUP BY role;

SELECT name
FROM users
WHERE role = "Data Engineer";

9️⃣ Create Analytics Index

Analytics indexes are schema-aware, so declare types explicitly:

CREATE INDEX idx_users_role
ON users(role:string);


Verify:

SELECT IndexName FROM Metadata.`Index`
WHERE DatasetName = "users";

🔍 Inspect Analytics Metadata
SELECT DataverseName FROM Metadata.`Dataverse`;
SELECT * FROM Metadata.`Index`;