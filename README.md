# Projet_2
Projet SQL Server : Gestion des Produits, Clients, Ventes et Employés


Introduction
Ce projet a pour objectif la création d'une base de données sous SQL Server permettant de gérer efficacement les produits, les clients, les ventes et les employés d'une entreprise commerciale


Objectifs du projet
•	Concevoir une base de données relationnelle fiable
•	Modéliser les entités et leurs relations
•	Implémenter la structure physique (tables, clés, intégrité)
•	Automatiser les processus (vues, procédures, triggers)
•	Mettre en place une stratégie de sauvegarde

Script du Projet
-- Création d'une base de Données Projet--
-- Script--
CREATE DATABASE Projet;
USE Projet;

--Creation des tables de notre base de données Projet--

 CREATE TABLE Client (
    id_client INT PRIMARY KEY IDENTITY (1,1),
    nom VARCHAR(50),
    prenom VARCHAR(50),
    email VARCHAR(100),
    telephone NVARCHAR(20)
);

CREATE TABLE Employe (
    id_employe INT PRIMARY KEY,
    nom VARCHAR(50),
    prenom VARCHAR(50),
    poste VARCHAR(50)
);

CREATE TABLE Produit (
    id_produit INT PRIMARY KEY,
    nom VARCHAR(100),
    prix DECIMAL(10,2),
    stock INT,
    categorie VARCHAR(50)
);

CREATE TABLE Vente (
    id_vente INT PRIMARY KEY,
    date_vente DATE,
    id_client INT FOREIGN KEY REFERENCES Client(id_client),
    id_employe INT FOREIGN KEY REFERENCES Employe(id_employe)
);

CREATE TABLE Detail_Vente (
    id_vente INT,
    id_produit INT,
    quantite INT,
    prix_unitaire DECIMAL(10,2),
    PRIMARY KEY (id_vente, id_produit),
    FOREIGN KEY (id_vente) REFERENCES Vente(id_vente),
    FOREIGN KEY (id_produit) REFERENCES Produit(id_produit)
);

--Visionnez les table créer--

select * from Client;
select * from Employe;
select * from Produit;
select * from Vente;
select * from Detail_Vente;

--Insertion des donnees--
INSERT INTO Client
values 
('Samuel','Jules','jules@gmail.com','065141820');
INSERT INTO Client
values 
('Mila', 'Fila', 'fila@gmail.com','065141824'),
('Megumi', 'Fifi', 'fifi@gmail.com','065551820'),
('Takeshi', 'jone', 'Jone@gmail.com','065251820'),
('Rita', 'Alice', 'Alice@gmail.com','065555620'),
('Tomas','Alfredson','Al@gmail.com','065651820'),
('Paul','Anderson','Anderson@gmail.com','065654820'),
('Wes','Anderson','Wes@gmail.com','065651860'),
('Richard','Ayoade','Richard@gmail.com','065658080'),
('Luc','Besson','Luc@gmail.com','068658088');

-- insertion dans la table employer

INSERT INTO Employe
values 
(101,'Bruce','Hardy','Manager'),
(102,'George','Vans','Manager'),
(103,'Tendresse','Tendre','Caissiere'),
(104,'Jessica','Jessi','Caissiere'),
(105,'Bel','Ange','Caissiere'),
(106,'Dalia','Dada','Caissiere');

--Insertion dans la table produit

INSERT INTO Produit
VALUES
(201,'Play 5', 300000, 9,'Jeux'),
(202,'IPhone 16 PRO', 250000, 4,'Telephone'),
(203,'Porte feuille', 15000, 3,'Portable'),
(204,'ITEL TELEPHONE', 13500, 3,'Telephone'),
(205,'TABLE A MANGER', 175000, 1,'Meuble');


--Insertion dans la ventes

INSERT INTO Vente 
VALUES 
(301, '2025-04-10', 1, 101),
(302, '2025-04-11', 2, 102),
(303, '2025-04-12', 3, 101),
(304, '2025-04-13', 1, 106),
(305, '2025-04-14', 3, 102);


INSERT INTO Vente 
VALUES 
(306, '2025-04-15', 6, 104),
(307, '2025-04-15', 7, 102),
(308, '2025-04-16', 8, 103),
(309, '2025-04-16', 9, 102),
(310, '2025-04-17', 1, 101),
(311, '2025-04-17', 1, 102),
(312, '2025-04-18', 2, 103),
(313, '2025-04-18', 3, 101),
(314, '2025-04-19', 4, 104),
(315, '2025-04-20', 5, 102),
(316, '2025-04-20', 6, 103),
(317, '2025-04-21', 7, 106),
(318, '2025-04-21', 8, 104),
(319, '2025-04-22', 9, 102),
(320, '2025-04-22', 10, 103);

--INSERTIONs a table details des ventes

INSERT INTO Detail_Vente(id_vente, id_produit, quantite, prix_unitaire)
VALUES
(303, 201, 1,300000),
  (313, 204, 2, 135000),
  (304, 205, 5, 150000),
  (305, 202,2, 250000),
  (314, 205,1,175000);


-- Vue 1 : Ventes détaillées avec informations client, produit et employé      
Cette vue rassemble toutes les informations d’une vente, incluant :

le client,

les produits achetés,

 l’employé ayant réalisé la vente.                          


CREATE VIEW V_Vente_Complete AS
SELECT 
    v.id_vente,
    v.date_vente,
    c.nom AS nom_client,
    c.prenom AS prenom_client,
    c.email,
    c.telephone,
    e.nom AS nom_employe,
    e.prenom AS prenom_employe,
    e.poste,
    p.nom AS nom_produit,
    p.categorie,
    dv.quantite,
    dv.prix_unitaire,
    (dv.quantite * dv.prix_unitaire) AS montant_total
FROM Vente v
JOIN Client c ON v.id_client = c.id_client
JOIN Employe e ON v.id_employe = e.id_employe
JOIN Detail_Vente dv ON v.id_vente = dv.id_vente
JOIN Produit p ON dv.id_produit = p.id_produit;


 REATE VIEW V_Resume_Produit AS
SELECT 
    p.id_produit,
    p.nom AS nom_produit,
    SUM(dv.quantite) AS quantite_totale_vendue,
    SUM(dv.quantite * dv.prix_unitaire) AS chiffre_affaire
FROM Produit p
JOIN Detail_Vente dv ON p.id_produit = dv.id_produit
GROUP BY p.id_produit, p.nom;


https://docs.google.com/presentation/d/1jR3Zmg9Uz69EvYAlrfWzq74052LyKNsT2ZQwwgBA35A/edit?usp=sharing


--Voici une procédure stockée simple pour ajouter un employé :

CREATE PROCEDURE AjouterEmploye
    @id_employe INT,
    @nom NVARCHAR(50),
    @prenom NVARCHAR(50),
    @poste NVARCHAR(50)
AS
BEGIN
    INSERT INTO Employe (id_employe, nom, prenom, poste)
    VALUES (@id_employe, @nom, @prenom, @poste);
END;

--Exécution de la procédure :


EXEC AjouterEmploye 
    @id_employe = 107, 
    @nom = 'Diallo', 
    @prenom = 'Amadou', 
    @poste = 'Technicien';

	select * from Employe;

	--TRIGGER pour la table Produi

	select * from Produit;

--Idée de trigger :
--Chaque fois qu’un produit est vendu (donc qu’une ligne est insérée dans Detail_Vente), on veut mettre à jour automatiquement le stock du produit dans la table Produit.

CREATE TRIGGER tr_update_stock
ON Detail_Vente
AFTER INSERT
AS
BEGIN
    UPDATE p
    SET p.stock = p.stock - i.quantite
    FROM Produit p
    JOIN inserted i ON p.id_produit = i.id_produit
END;

--Utilisation automatique (test du trigger)

INSERT INTO Detail_Vente (id_vente, id_produit, quantite, prix_unitaire)
VALUES (301, 203, 2, 15000);

select * from vente;
 --Sauvegardes automatisées
	--Script de sauvegarde planifié via SQL Server Agent
BACKUP DATABASE Projet
TO DISK = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Backup\Projet.bak'
WITH FORMAT, NAME = 'Full Backup';

--	Planification : quotidienne, hebdomadaire selon la criticité
