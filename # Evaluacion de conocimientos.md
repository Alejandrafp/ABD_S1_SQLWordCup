# Evaluacion de conocimientos

--Administracion Base de Datos
--Catedratico: Ivan Cruz 
--Alumno: Maria Alejandra Funez Perez 
--N° Cuenta: 61951373
--Fecha: 16 de enero de 2022

## Codigo


    --Creacion de Base de datos
    CREATE DATABASE MAFPWorldCup
    GO
    USE MAFPWorldCup
    
    --Creacion de tablas
    CREATE TABLE teams (
      team_id integer NOT NULL,
      team_name varchar(30) NOT NULL,
      UNIQUE (team_id)
    );
    
    CREATE TABLE matches (
      match_id integer NOT NULL,
      host_team integer NOT NULL,
      guest_team integer NOT NULL,
      host_goals integer NOT NULL,
      guest_goals integer NOT NULL,
      UNIQUE (match_id)
    );
    
    GO
    
    --Insertar registros
    INSERT [dbo].[matches] ([match_id], [host_team], [guest_team], [host_goals], [guest_goals])
      VALUES (1, 30, 20, 1, 0)
    GO
    INSERT [dbo].[matches] ([match_id], [host_team], [guest_team], [host_goals], [guest_goals])
      VALUES (2, 10, 20, 1, 2)
    GO
    INSERT [dbo].[matches] ([match_id], [host_team], [guest_team], [host_goals], [guest_goals])
      VALUES (3, 20, 50, 2, 2)
    GO
    INSERT [dbo].[matches] ([match_id], [host_team], [guest_team], [host_goals], [guest_goals])
      VALUES (4, 10, 30, 1, 0)
    GO
    INSERT [dbo].[matches] ([match_id], [host_team], [guest_team], [host_goals], [guest_goals])
      VALUES (5, 30, 50, 0, 1)
    GO
    INSERT [dbo].[teams] ([team_id], [team_name])
      VALUES (10, 'Give')
    GO
    INSERT [dbo].[teams] ([team_id], [team_name])
      VALUES (20, 'Never')
    GO
    INSERT [dbo].[teams] ([team_id], [team_name])
      VALUES (30, 'You')
    GO
    INSERT [dbo].[teams] ([team_id], [team_name])
      VALUES (40, 'Up')
    GO
    INSERT [dbo].[teams] ([team_id], [team_name])
      VALUES (50, 'Gonna')
    
    --Consulta
    SELECT
      team_id,
      team_name,
      COALESCE(SUM(CASE
        WHEN team_id = host_team THEN (
          CASE
            WHEN host_goals > guest_goals THEN 3
            WHEN host_goals = guest_goals THEN 1
            WHEN host_goals < guest_goals THEN 0
          END
          )
        WHEN team_id = guest_team THEN (
          CASE
            WHEN guest_goals > host_goals THEN 3
            WHEN guest_goals = host_goals THEN 1
            WHEN guest_goals < host_goals THEN 0
          END
          )
      END), 0) AS num_points
    FROM Teams
    LEFT JOIN Matches
      ON Teams.team_id = Matches.host_team
      OR Teams.team_id = Matches.guest_team
    GROUP BY team_id,
             team_name
    ORDER BY num_points DESC, team_id;

## Resultado



|team_id |team_name |num_points  |
|--------|----------|------------|
|20|Never|4|
|50|Gonna|4|
|10|Give|3|
|30|You|3|
|40|Up|0|


